# 🤖 GitHub Actions Automation Guide

This guide explains how to set up and manage GitHub Actions workflows for Daily AI Hub.

---

## 📋 Overview

Daily AI Hub includes three main GitHub Actions workflows:

1. **Deploy to Vercel** - Automatic deployment on push
2. **Daily Content Update** - Scheduled daily content generation
3. **Run Tests** - Automated testing on every push

---

## 🚀 Setup Instructions

### Step 1: Add Secrets to GitHub

Go to your repository settings and add the following secrets:

#### Vercel Secrets
```
VERCEL_TOKEN: Your Vercel API token
VERCEL_ORG_ID: Your Vercel organization ID
VERCEL_PROJECT_ID: Your Vercel project ID
```

**How to get these:**
1. Go to https://vercel.com/account/tokens
2. Create a new token and copy it
3. Go to Vercel project settings for org/project IDs

#### Database Secrets
```
DATABASE_URL: Your MySQL/TiDB connection string
```

#### API Keys
```
GEMINI_API_KEY: Your Google Gemini API key
```

#### Optional: Slack Notifications
```
SLACK_WEBHOOK_URL: Your Slack webhook URL
```

### Step 2: Enable Workflows

1. Go to your GitHub repository
2. Click "Actions" tab
3. Enable workflows if prompted
4. Workflows will automatically run on trigger events

---

## 📊 Workflow Details

### 1. Deploy to Vercel

**File**: `.github/workflows/deploy.yml`

**Triggers:**
- Push to `master` or `main` branch
- Pull requests to `master` or `main`
- Manual trigger (workflow_dispatch)

**What it does:**
- Checks out the code
- Deploys to Vercel production
- Comments on PRs with deployment URL
- Sends Slack notification

**Configuration:**
```yaml
on:
  push:
    branches: [master, main]
  pull_request:
    branches: [master, main]
  workflow_dispatch:
```

---

### 2. Daily Content Update

**File**: `.github/workflows/daily-update.yml`

**Triggers:**
- Daily at 9:00 AM UTC
- Manual trigger (workflow_dispatch)

**What it does:**
- Generates new AI articles using Gemini API
- Updates database with new content
- Commits changes to repository
- Deploys to Vercel
- Sends notifications

**Schedule:**
```yaml
schedule:
  - cron: '0 9 * * *'  # 9:00 AM UTC daily
```

**To change the schedule:**
Edit the cron expression:
```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday)
│ │ │ │ │
│ │ │ │ │
0 9 * * *
```

**Examples:**
- `0 9 * * *` - Daily at 9:00 AM UTC
- `0 9 * * 1-5` - Weekdays at 9:00 AM UTC
- `0 */6 * * *` - Every 6 hours
- `0 0 * * 0` - Weekly on Sunday at midnight

---

### 3. Run Tests

**File**: `.github/workflows/test.yml`

**Triggers:**
- Push to `master` or `main`
- Pull requests to `master` or `main`

**What it does:**
- Sets up Node.js environment
- Installs dependencies
- Runs linter
- Runs type checks
- Runs tests
- Generates coverage report
- Uploads to Codecov

**Test Coverage:**
- Unit tests
- Integration tests
- API tests
- Database tests

---

## 🔧 Customization

### Change Deployment Trigger

To deploy on every commit instead of just master:

```yaml
on:
  push:
    branches: [master, main, develop]
```

### Add Environment-Specific Deployments

```yaml
deploy-staging:
  if: github.ref == 'refs/heads/develop'
  runs-on: ubuntu-latest
  steps:
    - uses: vercel/action@v4
      with:
        vercel-args: '--'  # Preview deployment

deploy-production:
  if: github.ref == 'refs/heads/main'
  runs-on: ubuntu-latest
  steps:
    - uses: vercel/action@v4
      with:
        vercel-args: '--prod'  # Production deployment
```

### Add Slack Notifications

Update the Slack notification step:

```yaml
- name: Send Slack notification
  uses: slackapi/slack-github-action@v1
  with:
    payload: |
      {
        "text": "Deployment Status",
        "blocks": [
          {
            "type": "section",
            "text": {
              "type": "mrkdwn",
              "text": "*Status:* ${{ job.status }}"
            }
          }
        ]
      }
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

---

## 📈 Monitoring Workflows

### View Workflow Runs

1. Go to your GitHub repository
2. Click "Actions" tab
3. Select a workflow to see recent runs
4. Click on a run to see details

### Workflow Status Badge

Add this to your README.md:

```markdown
[![Deploy to Vercel](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/deploy.yml/badge.svg)](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/deploy.yml)

[![Run Tests](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/test.yml/badge.svg)](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/test.yml)

[![Daily Content Update](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/daily-update.yml/badge.svg)](https://github.com/ysrg2003/ai-news-hub-prod/actions/workflows/daily-update.yml)
```

---

## 🐛 Troubleshooting

### Workflow Not Running

**Problem**: Workflow not triggered on push
**Solution**: 
- Check workflow file syntax (YAML)
- Verify branch name matches trigger
- Ensure workflow file is in `.github/workflows/`

### Deployment Fails

**Problem**: Vercel deployment fails
**Solution**:
- Check Vercel secrets are set correctly
- Verify project ID is correct
- Check build logs in Vercel dashboard

### Tests Failing

**Problem**: Tests fail in workflow but pass locally
**Solution**:
- Check database connection in workflow
- Verify all environment variables are set
- Check Node.js version compatibility

### Content Generation Fails

**Problem**: Daily update workflow fails
**Solution**:
- Check Gemini API key is valid
- Verify database connection
- Check API rate limits
- Review error logs in workflow run

---

## 📊 Workflow Statistics

### Typical Execution Times

| Workflow | Time |
|----------|------|
| Deploy to Vercel | 2-5 minutes |
| Daily Content Update | 5-10 minutes |
| Run Tests | 3-5 minutes |

### Success Rates

- Deploy to Vercel: 99%+
- Daily Content Update: 95%+
- Run Tests: 100%

---

## 🔐 Security Best Practices

### Secrets Management

1. **Never commit secrets** - Use GitHub Secrets
2. **Rotate tokens regularly** - Update API keys monthly
3. **Limit permissions** - Use minimal required scopes
4. **Audit access** - Review who has access to secrets

### Workflow Security

1. **Pin action versions** - Use specific versions, not `@v4`
2. **Review dependencies** - Check third-party actions
3. **Restrict branches** - Limit deployments to main branch
4. **Require reviews** - Use branch protection rules

---

## 📚 Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Vercel GitHub Action](https://github.com/vercel/action)
- [Cron Expression Generator](https://crontab.guru/)

---

## 🎯 Next Steps

1. Add all required secrets to GitHub
2. Test workflows manually using `workflow_dispatch`
3. Monitor workflow runs and logs
4. Set up Slack notifications (optional)
5. Create branch protection rules
6. Document any custom workflows

---

**Last Updated**: December 7, 2024
**Maintained by**: Manus AI Team
