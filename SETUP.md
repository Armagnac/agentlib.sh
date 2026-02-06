# Setup Guide for agentlib.sh

This guide explains how to configure the sync system between this repository and agentlib.sh-web.

## Prerequisites

1. GitHub repository for agentlib.sh (this repo)
2. agentlib.sh-web deployed on Vercel
3. Supabase project set up

## Step 1: Generate Webhook Secret

Generate a random secret for authenticating webhook calls:

```bash
openssl rand -hex 32
```

Save this value - you'll need it for both GitHub and Vercel.

## Step 2: Configure Vercel Environment Variables

Add these environment variables to your Vercel project:

Go to: Vercel Dashboard → Your Project → Settings → Environment Variables

```env
# Webhook authentication secret (from Step 1)
SYNC_WEBHOOK_SECRET=your-generated-secret-here

# Supabase service role key (get from Supabase dashboard)
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key
```

**Important**: The service role key should already exist from your initial setup. Find it in:
Supabase Dashboard → Settings → API → service_role key

## Step 3: Configure GitHub Secrets

Add these secrets to this GitHub repository:

Go to: GitHub Repository → Settings → Secrets and variables → Actions → New repository secret

### SYNC_WEBHOOK_URL

Value: Your production webhook URL
```
https://agentlib.sh/api/sync
```

Or for testing with a staging environment:
```
https://your-staging-url.vercel.app/api/sync
```

### SYNC_WEBHOOK_SECRET

Value: The same secret you generated in Step 1 and added to Vercel

## Step 4: Update API Route Configuration

Edit `agentlib.sh-web/app/api/sync/route.ts` and update:

```typescript
const GITHUB_OWNER = 'YOUR_GITHUB_ORG'  // Replace with your GitHub org/username
```

## Step 5: Run Database Migration

In the agentlib.sh-web repository, run the migration:

```bash
# If using Supabase CLI locally
supabase db push

# Or manually via Supabase Dashboard:
# SQL Editor → New query → Paste contents of:
# supabase/migrations/004_add_git_sync_tracking.sql
```

## Step 6: Test the Sync

### Option A: Trigger via Push

1. Make a change to any file in `resources/`
2. Commit and push to main branch
3. Check GitHub Actions tab for workflow run
4. Verify resources appear on agentlib.sh

### Option B: Manual Trigger

Use GitHub's workflow_dispatch:

1. Go to: Actions → Trigger Web Sync → Run workflow
2. Click "Run workflow" on main branch
3. Check workflow logs for results

### Option C: Test Locally with curl

```bash
curl -X POST "http://localhost:3000/api/sync" \
  -H "Authorization: Bearer YOUR_SYNC_WEBHOOK_SECRET" \
  -H "Content-Type: application/json" \
  -d '{"ref": "refs/heads/main", "sha": "test"}'
```

## Troubleshooting

### Sync fails with 401 Unauthorized

- Check that `SYNC_WEBHOOK_SECRET` matches in both GitHub and Vercel
- Verify the Authorization header format: `Bearer SECRET`

### Sync fails with 500 Error

- Check Vercel function logs
- Verify `SUPABASE_SERVICE_ROLE_KEY` is set correctly
- Ensure database migration was applied

### Resources not appearing on website

- Check that resources have both `content.md` and `metadata.yaml`
- Verify `slug` in metadata is unique
- Check Supabase logs for upsert errors
- Confirm `status` is set to `'approved'`

### GitHub Action fails

- Check that both secrets are configured in GitHub
- Verify webhook URL is correct
- Check workflow logs for detailed error messages

## Monitoring

### View Sync Logs

**Vercel Function Logs**:
- Vercel Dashboard → Your Project → Logs
- Filter by `/api/sync`

**GitHub Action Logs**:
- GitHub Repository → Actions → Select workflow run

### Check Synced Resources

Query Supabase directly:

```sql
-- See all git-synced resources
SELECT slug, name, git_path, updated_at 
FROM resources 
WHERE source = 'git' 
ORDER BY updated_at DESC;

-- Count by platform
SELECT platform, COUNT(*) 
FROM resources 
WHERE source = 'git' 
GROUP BY platform;
```

## Security Notes

1. **Never commit secrets**: Use GitHub Secrets and Vercel Environment Variables
2. **Service role key**: Only use in server-side code, never in client
3. **Webhook secret**: Should be cryptographically random (32+ bytes)
4. **Public repository**: This repo can be public - it contains no secrets

## Next Steps

After setup is complete:

1. ✅ Add your first resources to `resources/`
2. ✅ Push to main and verify sync works
3. ✅ Invite contributors
4. ✅ Monitor sync logs for issues
5. ✅ Update README.md with your GitHub org name

## Need Help?

- Check [SYNC_ARCHITECTURE.md](https://github.com/YOUR_ORG/agentlib.sh-web/blob/main/SYNC_ARCHITECTURE.md) for technical details
- Open an issue in agentlib.sh-web repository
- Review Vercel and Supabase logs
