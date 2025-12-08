# 🤖 Automation Setup Guide - Daily AI Hub

This guide provides step-by-step instructions to set up GitHub Actions automation for Daily AI Hub.

---

## 📋 Prerequisites

Before setting up automation, ensure you have:

- ✅ GitHub repository: https://github.com/ysrg2003/ai-news-hub-prod
- ✅ Vercel project created
- ✅ Google Gemini API key
- ✅ Database connection string
- ✅ GitHub Personal Access Token

---

## 🚀 Setup Steps

### Step 1: Add GitHub Secrets

**Location**: Repository Settings → Secrets and variables → Actions

#### 1.1 Vercel Secrets

**VERCEL_TOKEN**
- Go to: https://vercel.com/account/tokens
- Create a new token
- Copy and paste into GitHub Secrets

**VERCEL_ORG_ID**
- Go to: https://vercel.com/account/settings
- Find "Team ID" or "Organization ID"
- Copy and paste into GitHub Secrets

**VERCEL_PROJECT_ID**
- Go to: https://vercel.com/dashboard
- Select your project
- Go to Settings → General
- Copy "Project ID"
- Paste into GitHub Secrets

#### 1.2 Database Secret

**DATABASE_URL**
```
mysql://username:password@host:port/database
```

Example:
```
mysql://root:password123@db.example.com:3306/ai_news_hub
```

#### 1.3 API Keys

**GEMINI_API_KEY**
- Go to: https://ai.google.dev/
- Create API key
- Copy and paste into GitHub Secrets

#### 1.4 Optional: Slack Webhook

**SLACK_WEBHOOK_URL** (optional)
- Go to: https://api.slack.com/apps
- Create new app
- Enable Incoming Webhooks
- Create webhook for your channel
- Copy and paste into GitHub Secrets

---

### Step 2: Verify Workflows

**Location**: Repository → Actions

1. Click "Actions" tab
2. You should see three workflows:
   - ✅ Deploy to Vercel
   - ✅ Daily Content Update
   - ✅ Run Tests

3. If workflows don't appear:
   - Go to Settings → Actions → General
   - Enable "Allow all actions and reusable workflows"

---

### Step 3: Test Workflows Manually

#### Test Deploy Workflow

1. Go to Actions tab
2. Select "Deploy to Vercel"
3. Click "Run workflow"
4. Select branch: `master`
5. Click "Run workflow"
6. Wait for completion (2-5 minutes)

#### Test Content Update Workflow

1. Go to Actions tab
2. Select "Daily Content Update"
3. Click "Run workflow"
4. Select branch: `master`
5. Click "Run workflow"
6. Wait for completion (5-10 minutes)

#### Test Test Workflow

1. Go to Actions tab
2. Select "Run Tests"
3. Click "Run workflow"
4. Select branch: `master`
5. Click "Run workflow"
6. Wait for completion (3-5 minutes)

---

### Step 4: Configure Workflow Schedules

#### Daily Content Generation

**Current Schedule**: 9:00 AM UTC daily

**To change the schedule:**

1. Open `.github/workflows/daily-update.yml`
2. Find the `schedule` section:
   ```yaml
   schedule:
     - cron: '0 9 * * *'
   ```

3. Edit the cron expression:
   - `0 9 * * *` = Daily at 9:00 AM UTC
   - `0 9 * * 1-5` = Weekdays at 9:00 AM UTC
   - `0 */6 * * *` = Every 6 hours
   - `0 0 * * 0` = Weekly on Sunday at midnight

4. Commit and push changes

---

## 📊 Monitoring Workflows

### View Workflow Runs

1. Go to repository Actions tab
2. Click on a workflow name
3. See list of recent runs with status:
   - ✅ Success (green)
   - ❌ Failed (red)
   - ⏳ In progress (yellow)

### View Workflow Logs

1. Click on a specific run
2. Click on the job name
3. Expand steps to see detailed logs
4. Check for errors or warnings

### Set Up Notifications

**GitHub Notifications:**
1. Go to Settings → Notifications
2. Enable "Workflow runs"
3. Choose notification method (email, web)

**Slack Notifications:**
1. Add `SLACK_WEBHOOK_URL` secret
2. Workflows will automatically post to Slack

---

## 🔧 Workflow Configuration

### Deploy to Vercel Workflow

**Triggers:**
- Push to `master` or `main` branch
- Pull requests to `master` or `main`
- Manual trigger (workflow_dispatch)

**What it does:**
1. Checks out code
2. Deploys to Vercel production
3. Comments on PRs with deployment URL
4. Sends Slack notification

**Configuration file:** `.github/workflows/deploy.yml`

### Daily Content Update Workflow

**Triggers:**
- Daily at 9:00 AM UTC
- Manual trigger (workflow_dispatch)

**What it does:**
1. Generates new articles using Gemini API
2. Updates database
3. Commits changes
4. Deploys to Vercel
5. Sends notifications

**Configuration file:** `.github/workflows/daily-update.yml`

### Run Tests Workflow

**Triggers:**
- Push to `master` or `main`
- Pull requests to `master` or `main`

**What it does:**
1. Installs dependencies
2. Runs linter
3. Runs type checks
4. Runs tests
5. Generates coverage report
6. Uploads to Codecov

**Configuration file:** `.github/workflows/test.yml`

---

## 🐛 Troubleshooting

### Workflow Not Triggering

**Problem**: Workflow doesn't run on push
**Solution**:
1. Check workflow file is in `.github/workflows/`
2. Verify YAML syntax is correct
3. Check branch name matches trigger
4. Ensure workflows are enabled in Settings

### Deployment Fails

**Problem**: Vercel deployment fails
**Solution**:
1. Check Vercel secrets are correct
2. Verify project ID is correct
3. Check build logs in Vercel dashboard
4. Review GitHub Actions logs

### Content Generation Fails

**Problem**: Daily update fails
**Solution**:
1. Check Gemini API key is valid
2. Verify database connection
3. Check API rate limits
4. Review error logs

### Tests Fail in Workflow

**Problem**: Tests pass locally but fail in workflow
**Solution**:
1. Check database is running in workflow
2. Verify all environment variables are set
3. Check Node.js version compatibility
4. Review test logs

---

## 📈 Performance Optimization

### Reduce Workflow Time

1. **Cache dependencies:**
   ```yaml
   - uses: actions/setup-node@v4
     with:
       cache: 'npm'
   ```

2. **Parallel jobs:**
   ```yaml
   jobs:
     test:
       runs-on: ubuntu-latest
     lint:
       runs-on: ubuntu-latest
   ```

3. **Skip unnecessary steps:**
   ```yaml
   - name: Step name
     if: github.event_name == 'push'
   ```

### Reduce Costs

1. Use `ubuntu-latest` (cheaper than macOS/Windows)
2. Limit concurrent jobs
3. Cache dependencies
4. Skip tests for documentation changes

---

## 🔐 Security Best Practices

### Secrets Management

1. **Never commit secrets** - Use GitHub Secrets only
2. **Rotate tokens** - Update API keys monthly
3. **Limit permissions** - Use minimal required scopes
4. **Audit access** - Review who has access

### Workflow Security

1. **Pin action versions** - Use specific versions
2. **Review dependencies** - Check third-party actions
3. **Restrict branches** - Limit deployments to main
4. **Require reviews** - Use branch protection rules

### Example: Branch Protection

1. Go to Settings → Branches
2. Add rule for `master` branch
3. Enable "Require status checks to pass"
4. Select "Run Tests" workflow
5. Enable "Dismiss stale pull request approvals"

---

## 📚 Useful Commands

### Manually Trigger Workflow

```bash
# Using GitHub CLI
gh workflow run deploy.yml --ref master

# Using curl
curl -X POST \
  -H "Authorization: token YOUR_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/ysrg2003/ai-news-hub-prod/actions/workflows/deploy.yml/dispatches \
  -d '{"ref":"master"}'
```

### View Workflow Status

```bash
# Using GitHub CLI
gh run list --workflow=deploy.yml

# View specific run
gh run view RUN_ID
```

### View Logs

```bash
# Using GitHub CLI
gh run view RUN_ID --log
```

---

## 📋 Checklist - Automation Setup

- [ ] Add VERCEL_TOKEN to GitHub Secrets
- [ ] Add VERCEL_ORG_ID to GitHub Secrets
- [ ] Add VERCEL_PROJECT_ID to GitHub Secrets
- [ ] Add DATABASE_URL to GitHub Secrets
- [ ] Add GEMINI_API_KEY to GitHub Secrets
- [ ] (Optional) Add SLACK_WEBHOOK_URL to GitHub Secrets
- [ ] Verify workflows appear in Actions tab
- [ ] Test Deploy workflow manually
- [ ] Test Daily Content Update workflow manually
- [ ] Test Run Tests workflow manually
- [ ] Set up GitHub notifications
- [ ] (Optional) Set up Slack notifications
- [ ] Configure workflow schedules
- [ ] Set up branch protection rules
- [ ] Document any custom configurations

---

## 🎯 Next Steps

1. **Complete Setup**
   - Add all required secrets
   - Test workflows manually
   - Set up notifications

2. **Monitor Workflows**
   - Check workflow runs regularly
   - Review logs for errors
   - Monitor deployment status

3. **Optimize Workflows**
   - Reduce execution time
   - Improve error handling
   - Add additional checks

4. **Extend Automation**
   - Add more workflows as needed
   - Integrate with other tools
   - Create custom actions

---

## 📞 Support

For issues or questions:

1. Check GitHub Actions documentation
2. Review workflow logs
3. Check GitHub Status page
4. Open an issue on GitHub

---

## 📚 Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Vercel GitHub Action](https://github.com/vercel/action)
- [Cron Expression Generator](https://crontab.guru/)

---

**Last Updated**: December 7, 2024
**Maintained by**: Manus AI Team

---

## 🎉 Congratulations!

Your GitHub Actions automation is now set up! Your Daily AI Hub will:

- ✅ Deploy automatically on every push
- ✅ Generate new content daily at 9:00 AM UTC
- ✅ Run tests automatically
- ✅ Send notifications on success/failure

Happy automating! 🚀
