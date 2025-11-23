# LinguaFlow Backend - Deployment Guide

## 🚀 Production Deployment Strategy

### Overview
This guide covers deploying LinguaFlow TCF Backend to production using GitHub Actions CI/CD, Docker containerization, and cloud hosting platforms.

## 🔄 CI/CD Pipeline Overview

### GitHub Actions Workflow
**Location**: `.github/workflows/` directory

**Automated Checks**:
- Lint code (ESLint)
- Type check (TypeScript)
- Run unit tests (Jest)
- Build Docker image
- Push to registry
- Deploy to staging/production

### Workflow Triggers
- `push` to main branch → Deploy to production
- `push` to develop branch → Deploy to staging
- `pull_request` → Run tests only
- `workflow_dispatch` → Manual trigger

## 🐳 Docker Containerization

### Build Docker Image
```bash
# Development
docker build -f docker/Dockerfile.dev -t linguaflow-backend:dev .

# Production (multi-stage)
docker build -f docker/Dockerfile -t linguaflow-backend:latest .
docker tag linguaflow-backend:latest linguaflow-backend:1.0.0
```

### Run Container
```bash
# Development
docker run -p 3000:3000 --env-file .env linguaflow-backend:dev npm run dev

# Production
docker run -p 3000:3000 --env-file .env linguaflow-backend:latest npm start
```

## 🌐 Hosting Platforms (Recommended)

### 1. Render.com (Recommended)
**Advantages**:
- Free tier available
- Automatic deployments from GitHub
- Built-in PostgreSQL/MongoDB support
- Zero-downtime deploys
- Free SSL/TLS

**Setup**:
```
1. Connect GitHub repository
2. Select Web Service
3. Configure build command: npm run build
4. Configure start command: npm start
5. Add environment variables
6. Deploy
```

### 2. Railway.app
**Advantages**:
- Simple GitHub integration
- Auto-deployments
- Built-in databases
- $5/month credit

**Setup**:
1. Connect GitHub
2. Select Node.js environment
3. Add MongoDB service
4. Deploy

### 3. AWS (EC2 + ECS)
**For production-grade infrastructure**:
- EC2 instances with auto-scaling
- Elastic Container Service (ECS)
- RDS for managed database
- S3 for file uploads
- CloudFront for CDN

## 📊 Database Setup

### MongoDB Atlas (Cloud)
```
1. Create cluster at mongodb.com/cloud
2. Create database user
3. Get connection string
4. Add to environment variables
```

**Connection String Format**:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/dbname?retryWrites=true&w=majority
```

### Local Development
```bash
# Start MongoDB
mongod

# Or use Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest
```

## 🔑 Environment Variables Setup

### Required for Production
```env
NODE_ENV=production
PORT=3000
MONGODB_URI=<production_mongodb_uri>
JWT_SECRET=<generate_strong_secret>
JWT_REFRESH_SECRET=<generate_strong_secret>
CORS_ORIGIN=https://yourdomain.com
LOG_LEVEL=warn
```

### Generate Secure Secrets
```bash
# Using Node.js
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"

# Using OpenSSL
openssl rand -hex 32
```

## 🚀 Deployment Steps

### Manual Deployment to Render

1. **Prepare Code**
```bash
git add .
git commit -m "chore: Prepare for production deployment"
git push origin main
```

2. **Connect Repository**
- Go to render.com
- Click "New +"
- Select "Web Service"
- Connect GitHub repository

3. **Configure Service**
- Name: linguaflow-tcf-backend
- Environment: Node
- Build Command: `npm install && npm run build`
- Start Command: `npm start`
- Instance Type: Free or Paid tier

4. **Add Environment Variables**
- Click "Environment"
- Add each variable from .env

5. **Deploy**
- Click "Create Web Service"
- Wait for build and deployment

### GitHub Actions Auto-Deployment

1. **Create Workflow File** `.github/workflows/deploy.yml`
2. **Configure Secrets**
   - Go to Settings > Secrets and variables > Actions
   - Add: RENDER_DEPLOY_HOOK, MONGODB_URI, JWT_SECRET, etc.
3. **Push Trigger**
   - Push to main → Auto-deploys to production

## 📈 Monitoring & Logging

### Health Check Endpoint
```
GET /api/health
Response: { "status": "ok", "uptime": 12345 }
```

### View Logs
- **Render**: Dashboard > Logs tab
- **Railway**: Railway Dashboard > Logs
- **Local**: `npm run dev` console output

## 🔒 Security Checklist

- [ ] All secrets in environment variables (never in code)
- [ ] HTTPS/SSL enabled
- [ ] CORS properly configured
- [ ] Rate limiting enabled
- [ ] Input validation on all endpoints
- [ ] MongoDB credentials secured
- [ ] JWT secrets strong and rotated
- [ ] Database backups scheduled
- [ ] Error messages don't leak sensitive info
- [ ] Dependencies updated and scanned

## 🔄 Continuous Integration Workflow

```
┌─ Code Push to GitHub
│
├─ GitHub Actions Triggered
│  ├─ Lint & Format Check
│  ├─ TypeScript Compilation
│  ├─ Unit Tests
│  └─ Integration Tests
│
├─ If develop branch
│  └─ Deploy to Staging
│
├─ If main branch (after PR merge)
│  ├─ Build Docker Image
│  ├─ Push to Registry
│  └─ Deploy to Production
│
└─ Monitor & Alert
```

## 📱 API Testing in Production

```bash
# Test health endpoint
curl https://yourdomain.com/api/health

# Test authentication
curl -X POST https://yourdomain.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password"}'
```

## 🚨 Troubleshooting

### Build Fails
- Check Node.js version (requires 18+)
- Verify all environment variables set
- Check npm dependencies resolution
- Review build logs

### Application Crashes
- Check MongoDB connection
- Verify JWT secrets set
- Review application logs
- Check disk space on server

### Database Connection Errors
- Verify MongoDB URI format
- Check network access rules
- Confirm database credentials
- Test local connection first

## 📚 Resources

- [Render Documentation](https://render.com/docs)
- [Railway Documentation](https://docs.railway.app)
- [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [GitHub Actions](https://docs.github.com/en/actions)

## 🔗 Next Steps

1. Set up MongoDB Atlas instance
2. Create Render account and connect GitHub
3. Configure environment variables
4. Deploy first version
5. Monitor logs and metrics
6. Set up automated backups
7. Configure custom domain
8. Enable CI/CD pipeline
