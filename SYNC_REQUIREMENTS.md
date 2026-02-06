# Sync Script Requirements for agentlib.sh-web

This document outlines the changes needed in the agentlib.sh-web repository to support frontmatter-only metadata.

## Overview

The sync script needs to be updated to:
1. Parse frontmatter from `content.md` files instead of reading separate `metadata.yaml` files
2. Auto-generate derived metadata fields using file paths and LLMs
3. Handle both old and new formats during transition (optional)

## Current Implementation

**File:** `app/api/sync/route.ts` (or equivalent)

**Current behavior:**
- Reads `content.md` for content
- Reads `metadata.yaml` for all metadata
- Combines both for database upsert

## Required Changes

### 1. Install Dependencies

Add frontmatter parsing library:

```bash
npm install gray-matter
```

### 2. Update File Reading Logic

**Before:**
```typescript
const contentPath = `resources/${platform}/${type}/${name}/content.md`;
const metadataPath = `resources/${platform}/${type}/${name}/metadata.yaml`;

const content = await fetchFile(contentPath);
const metadata = await fetchFile(metadataPath);
const parsedMeta = yaml.parse(metadata);
```

**After:**
```typescript
import matter from 'gray-matter';

const filePath = `resources/${platform}/${type}/${name}/content.md`;
const fileContent = await fetchFile(filePath);

// Parse frontmatter and content
const { data: frontmatter, content } = matter(fileContent);
```

### 3. Auto-generate Derived Fields

Implement auto-generation for fields not in frontmatter:

```typescript
// Extract slug from file path
const slug = name; // e.g., "quick-commit" from path

// Generate name from slug (titlecase)
const autoName = frontmatter.name || titleCase(slug);

// Generate tagline using LLM
const tagline = await generateTagline(frontmatter.description);

// Generate tags using LLM
const tags = await generateTags(frontmatter.description, content);
```

### 4. LLM Integration for Auto-generation

#### Tagline Generation

```typescript
async function generateTagline(description: string): Promise<string> {
  const prompt = `Given this description: "${description}"
Generate a concise one-line tagline (max 60 chars).
Return only the tagline, no quotes or formatting.`;

  const response = await anthropic.messages.create({
    model: 'claude-3-haiku-20240307',
    max_tokens: 100,
    messages: [{ role: 'user', content: prompt }]
  });

  return response.content[0].text.trim();
}
```

#### Tag Extraction

```typescript
async function generateTags(
  description: string,
  content: string
): Promise<{ technology: string[]; lifecycle: string[]; subject: string[] }> {
  const prompt = `Analyze this content and extract relevant tags:

Description: ${description}

Content (first 1000 chars): ${content.substring(0, 1000)}

Extract:
- technology tags (git, typescript, python, react, docker, etc.)
- lifecycle tags (development, testing, deployment, debugging)
- subject tags (commits, code-quality, security, performance)

Return as JSON: {"technology": [...], "lifecycle": [...], "subject": [...]}
Return ONLY valid JSON, no other text.`;

  const response = await anthropic.messages.create({
    model: 'claude-3-haiku-20240307',
    max_tokens: 200,
    messages: [{ role: 'user', content: prompt }]
  });

  return JSON.parse(response.content[0].text);
}
```

### 5. Build Resource Object

```typescript
const resource = {
  slug: slug,
  name: autoName,
  tagline: tagline,
  description: frontmatter.description,
  resource_type: frontmatter.resource_type,
  command_syntax: frontmatter.command_syntax || null,
  source_url: frontmatter.source_url || null,
  platform: platform,
  type: type,
  content: content,
  git_path: filePath,
  source: 'git',
  status: 'approved',
  tags: tags
};
```

### 6. Validation

Add validation for required frontmatter fields:

```typescript
function validateFrontmatter(frontmatter: any, resourceType: string) {
  const required = ['description', 'resource_type'];
  
  if (resourceType === 'command' && !frontmatter.command_syntax) {
    throw new Error('command_syntax required for commands');
  }
  
  if (resourceType === 'agent' && (!frontmatter.name || !frontmatter.tools)) {
    throw new Error('name and tools required for agents');
  }
  
  for (const field of required) {
    if (!frontmatter[field]) {
      throw new Error(`Missing required field: ${field}`);
    }
  }
}
```

### 7. Handle Migration (Optional)

Support both formats during transition:

```typescript
async function parseResource(path: string) {
  const contentPath = `${path}/content.md`;
  const metadataPath = `${path}/metadata.yaml`;
  
  const fileContent = await fetchFile(contentPath);
  
  // Try parsing as frontmatter first
  const { data: frontmatter, content } = matter(fileContent);
  
  if (Object.keys(frontmatter).length > 0) {
    // New format: use frontmatter
    return { frontmatter, content };
  } else {
    // Old format: try reading metadata.yaml
    try {
      const metadata = await fetchFile(metadataPath);
      const parsedMeta = yaml.parse(metadata);
      return { frontmatter: parsedMeta, content: fileContent };
    } catch (e) {
      throw new Error('No frontmatter or metadata.yaml found');
    }
  }
}
```

## Testing

### Test Cases

1. **Command with frontmatter**
   - File: `resources/cursor/commands/quick-commit/content.md`
   - Expected: Correctly parsed, auto-generated fields present

2. **Agent with full frontmatter**
   - File: `resources/claude-code/agents/code-reviewer/content.md`
   - Expected: All fields from frontmatter + auto-generated tags

3. **Skill with minimal frontmatter**
   - File: `resources/cursor/skills/pdf-processing/content.md`
   - Expected: Auto-generated name, tagline, tags

### Manual Testing

```bash
# Test locally with dev server
npm run dev

# Trigger sync manually
curl -X POST "http://localhost:3000/api/sync" \
  -H "Authorization: Bearer YOUR_SECRET" \
  -H "Content-Type: application/json" \
  -d '{"ref": "refs/heads/main", "sha": "test"}'

# Check Supabase for correct data
```

## Deployment Checklist

- [ ] Install `gray-matter` dependency
- [ ] Update sync route to parse frontmatter
- [ ] Implement auto-generation functions
- [ ] Add LLM API integration (Anthropic/OpenAI)
- [ ] Add validation for required fields
- [ ] Test with existing resources
- [ ] Update environment variables (add LLM API key)
- [ ] Deploy to production
- [ ] Verify sync works with test push
- [ ] Monitor logs for errors

## Environment Variables

Add to Vercel:

```env
ANTHROPIC_API_KEY=your-api-key-here
# or
OPENAI_API_KEY=your-api-key-here
```

## Cost Considerations

LLM auto-generation adds API costs:

- **Tagline generation**: ~100 tokens per resource (~$0.000025 per resource with Claude Haiku)
- **Tag extraction**: ~200 tokens per resource (~$0.00005 per resource with Claude Haiku)
- **Total per resource**: ~$0.000075 (less than 0.01 cents)

For 100 resources: ~$0.0075 per sync

**Optimization:**
- Cache generated taglines/tags in database
- Only regenerate if description changes
- Use cheaper models (Claude Haiku, GPT-3.5-turbo)

## Rollback Plan

If issues occur:

1. Keep old sync code in separate function
2. Add feature flag to switch between old/new parsing
3. Can revert to reading metadata.yaml files if needed

## Questions?

Contact the agentlib.sh team or open an issue in agentlib.sh-web repository.
