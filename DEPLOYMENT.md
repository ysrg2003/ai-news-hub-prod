# 🚀 Deployment Guide - Daily AI Hub

This guide provides comprehensive instructions for deploying Daily AI Hub to various platforms.

---

## 📌 Quick Start

### Vercel Deployment (Recommended)

**Step 1: Connect GitHub Repository**
```bash
1. Go to https://vercel.com/dashboard
2. Click "Add New" → "Project"
3. Select "Import Git Repository"
4. Choose "ysrg2003/ai-news-hub-prod"
5. Click "Import"
```

**Step 2: Configure Environment Variables**
```bash
1. In Vercel Dashboard, go to Settings → Environment Variables
2. Add the following variables:
```

| Variable | Value |
|----------|-------|
| `VITE_APP_TITLE` | `Daily AI Hub` |
| `VITE_APP_LOGO` | `https://files.manuscdn.com/user_upload_by_module/web_dev_logo/310519663216063405/bVJNzpuLdDhujigo.png` |
| `VITE_ANALYTICS_ENDPOINT` | `https://manus-analytics.com` |
| `VITE_ANALYTICS_WEBSITE_ID` | `c57d7b76-c70c-4acf-ab17-d63fe333a450` |

**Step 3: Deploy**
```bash
1. Click "Deploy"
2. Wait for build to complete (usually 2-3 minutes)
3. Your site will be live at: https://ai-news-hub-prod.vercel.app
```

---

## 🔧 Advanced Configuration

### Custom Domain Setup

**On Vercel:**
```
1. Go to Project Settings → Domains
2. Click "Add Domain"
3. Enter your custom domain (e.g., ai-news.example.com)
4. Follow DNS configuration instructions
5. Update DNS records at your domain provider
```

### Performance Optimization

**Caching Strategy:**
- Static assets: 1 year cache
- HTML: No cache (always fresh)
- API responses: 5 minutes cache

**Compression:**
- Gzip enabled for all text files
- Brotli compression for modern browsers

---

## 📊 Monitoring & Analytics

### Vercel Analytics

```
1. Go to Project Settings → Analytics
2. Enable "Web Analytics"
3. View real-time metrics:
   - Page views
   - Unique visitors
   - Response times
   - Error rates
```

### Custom Analytics

The platform includes integration with Umami Analytics:
- Real-time visitor tracking
- Page view analytics
- Event tracking
- Custom dashboards

---

## 🔄 Continuous Deployment

### GitHub Actions Integration

The repository includes automated workflows:

**Daily Content Generation:**
- Runs at 9:00 AM UTC
- Generates new articles
- Updates database
- Publishes to production

**Deployment Workflow:**
```yaml
name: Deploy to Vercel
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: vercel/action@v4
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

---

## 🛡️ Security Checklist

Before deploying to production:

- [ ] All environment variables are set
- [ ] Database credentials are secure
- [ ] API keys are not exposed
- [ ] HTTPS is enabled
- [ ] CORS is properly configured
- [ ] Rate limiting is enabled
- [ ] Security headers are set
- [ ] Content Security Policy is configured

---

## 📈 Scaling Considerations

### Database Optimization

```sql
-- Create indexes for better performance
CREATE INDEX idx_articles_category ON articles(category);
CREATE INDEX idx_articles_published ON articles(published_at);
CREATE INDEX idx_articles_slug ON articles(slug);
```

### Caching Strategy

```javascript
// Implement Redis caching
const redis = require('redis');
const client = redis.createClient();

// Cache popular articles
client.setex('articles:popular', 3600, JSON.stringify(articles));
```

### CDN Configuration

```
1. Enable Vercel Edge Network
2. Configure regional caching
3. Set up image optimization
4. Enable automatic compression
```

---

## 🔍 Troubleshooting

### Build Failures

**Issue**: Build fails with "Out of memory"
```bash
# Solution: Increase Node memory
NODE_OPTIONS=--max-old-space-size=4096 npm run build
```

**Issue**: Deployment timeout
```bash
# Solution: Optimize build process
- Remove unnecessary dependencies
- Enable code splitting
- Use dynamic imports
```

### Performance Issues

**Issue**: Slow page load times
```bash
# Solutions:
1. Enable image optimization
2. Implement lazy loading
3. Use code splitting
4. Minimize CSS/JS bundles
```

---

## 📞 Support & Monitoring

### Health Checks

```bash
# Check deployment status
curl -I https://ai-news-hub-prod.vercel.app

# Monitor response times
curl -w "@curl-format.txt" -o /dev/null -s https://ai-news-hub-prod.vercel.app
```

### Error Tracking

```
1. Enable Sentry integration
2. Configure error notifications
3. Set up alerting thresholds
4. Monitor error rates
```

---

## 🎯 Post-Deployment Tasks

### 1. SEO Verification
```bash
- Submit sitemap to Google Search Console
- Verify robots.txt
- Check meta tags
- Test structured data
```

### 2. Performance Testing
```bash
- Run Lighthouse audit
- Test Core Web Vitals
- Check mobile performance
- Verify caching headers
```

### 3. Security Audit
```bash
- Run security scan
- Check SSL certificate
- Verify CORS headers
- Test authentication
```

### 4. Monitoring Setup
```bash
- Configure uptime monitoring
- Set up error alerts
- Enable performance tracking
- Create dashboards
```

---

## 📋 Deployment Checklist

```
Pre-Deployment:
- [ ] All tests passing
- [ ] Code reviewed
- [ ] Environment variables configured
- [ ] Database migrations tested
- [ ] Backup created

Deployment:
- [ ] Build successful
- [ ] All routes working
- [ ] APIs responding
- [ ] Database connected
- [ ] Analytics tracking

Post-Deployment:
- [ ] Smoke tests passed
- [ ] Performance acceptable
- [ ] No error spikes
- [ ] Users can access site
- [ ] Monitoring active
```

---

## 🔗 Useful Links

- [Vercel Documentation](https://vercel.com/docs)
- [Next.js Deployment](https://nextjs.org/docs/deployment)
- [GitHub Actions](https://github.com/features/actions)
- [Sentry Error Tracking](https://sentry.io/)
- [Umami Analytics](https://umami.is/)

---

**Last Updated**: December 7, 2024
**Maintained by**: Manus AI Team
