# 🚀 Deployment Guide

## Overview

This guide covers deploying the AI Invoice Generator application to various platforms and environments. The application consists of a React frontend and Node.js backend with Supabase as the database.

**Created by**: Pranay Tyagi & Finn Connelly  
**Note**: This is a private project. The source code is not publicly available, but this documentation is provided for reference and understanding of the deployment architecture.

## Prerequisites

- Node.js 18+ installed
- Git installed
- Supabase account
- OpenAI API key
- Domain name (for production)

## Environment Setup

### 1. Environment Variables

Create environment files for different environments:

#### Development (.env.development)
```env
NODE_ENV=development
PORT=3000
SUPABASE_URL=your-supabase-url
SUPABASE_ANON_KEY=your-supabase-anon-key
OPENAI_API_KEY=your-openai-api-key
CORS_ORIGIN=http://localhost:5173
```

#### Production (.env.production)
```env
NODE_ENV=production
PORT=3000
SUPABASE_URL=your-production-supabase-url
SUPABASE_ANON_KEY=your-production-supabase-anon-key
OPENAI_API_KEY=your-openai-api-key
CORS_ORIGIN=https://your-domain.com
```

### 2. Supabase Configuration

1. **Create Supabase Project**
   - Go to [supabase.com](https://supabase.com)
   - Create new project
   - Note down URL and anon key

2. **Database Setup**
   ```sql
   -- Run the schema from root/database/schema.sql
   -- This creates all necessary tables and RLS policies
   ```

3. **Authentication Setup**
   - Configure email templates
   - Set up redirect URLs
   - Enable email verification

---

## 🏠 Local Development

### Quick Start

```bash
# Note: This is a private repository
# Contact the project maintainer for access to the source code

# Once you have access:
# Install dependencies
npm install
cd ai-invoice-generator-frontend && npm install
cd ../root && npm install

# Set up environment
cp root/env.example root/.env
# Edit .env with your credentials

# Start development servers
# Terminal 1: Backend
cd root && npm start

# Terminal 2: Frontend
cd ai-invoice-generator-frontend && npm run dev
```

### Development URLs
- Frontend: `http://localhost:5173`
- Backend: `http://localhost:3000`
- API: `http://localhost:3000/api`

---

## ☁️ Cloud Deployment Options

### Option 1: Vercel + Railway (Recommended)

#### Frontend Deployment (Vercel)

1. **Connect Repository**
   ```bash
   # Install Vercel CLI
   npm i -g vercel
   
   # Login to Vercel
   vercel login
   
   # Deploy from frontend directory
   cd ai-invoice-generator-frontend
   vercel
   ```

2. **Configure Build Settings**
   ```json
   {
     "buildCommand": "npm run build",
     "outputDirectory": "dist",
     "installCommand": "npm install"
   }
   ```

3. **Environment Variables**
   - Add in Vercel dashboard:
     - `VITE_API_URL`: Your backend URL
     - `VITE_SUPABASE_URL`: Your Supabase URL
     - `VITE_SUPABASE_ANON_KEY`: Your Supabase anon key

#### Backend Deployment (Railway)

1. **Connect Repository**
   ```bash
   # Install Railway CLI
   npm install -g @railway/cli
   
   # Login to Railway
   railway login
   
   # Deploy from root directory
   cd root
   railway init
   railway up
   ```

2. **Configure Environment Variables**
   ```bash
   railway variables set SUPABASE_URL=your-supabase-url
   railway variables set SUPABASE_ANON_KEY=your-supabase-anon-key
   railway variables set OPENAI_API_KEY=your-openai-api-key
   railway variables set NODE_ENV=production
   ```

### Option 2: Netlify + Heroku

#### Frontend Deployment (Netlify)

1. **Build Configuration**
   ```toml
   # netlify.toml
   [build]
     base = "ai-invoice-frontend"
     publish = "dist"
     command = "npm run build"
   
   [build.environment]
     NODE_VERSION = "18"
   ```

2. **Deploy**
   ```bash
   # Build and deploy
   cd ai-invoice-generator-frontend
   npm run build
   netlify deploy --prod --dir=dist
   ```

#### Backend Deployment (Heroku)

1. **Create Heroku App**
   ```bash
   # Install Heroku CLI
   # Create app
   heroku create your-app-name
   
   # Set environment variables
   heroku config:set SUPABASE_URL=your-supabase-url
   heroku config:set SUPABASE_ANON_KEY=your-supabase-anon-key
   heroku config:set OPENAI_API_KEY=your-openai-api-key
   heroku config:set NODE_ENV=production
   ```

2. **Deploy**
   ```bash
   # Deploy from root directory
   cd root
   git subtree push --prefix=root heroku main
   ```

### Option 3: AWS Deployment

#### Frontend (S3 + CloudFront)

1. **Build and Upload**
   ```bash
   # Build frontend
   cd ai-invoice-generator-frontend
   npm run build
   
   # Upload to S3
   aws s3 sync dist/ s3://your-bucket-name
   
   # Configure CloudFront distribution
   # Set up custom domain
   ```

#### Backend (EC2 + RDS)

1. **Launch EC2 Instance**
   ```bash
   # Launch Ubuntu 20.04 LTS instance
   # Install Node.js
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   
   # Install PM2 for process management
   npm install -g pm2
   ```

2. **Deploy Application**
   ```bash
   # Note: This is a private repository
   # Contact the project maintainer for access to the source code
   # Once you have access, navigate to the project directory
   cd ai-invoice-generator/root
   
   # Install dependencies
   npm install
   
   # Set environment variables
   export SUPABASE_URL=your-supabase-url
   export SUPABASE_ANON_KEY=your-supabase-anon-key
   export OPENAI_API_KEY=your-openai-api-key
   export NODE_ENV=production
   
   # Start with PM2
   pm2 start script.js --name "ai-invoice-generator"
   pm2 save
   pm2 startup
   ```

---

## 🐳 Docker Deployment

### Dockerfile for Backend

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Start application
CMD ["node", "script.js"]
```

### Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  backend:
    build: ./root
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - SUPABASE_URL=${SUPABASE_URL}
      - SUPABASE_ANON_KEY=${SUPABASE_ANON_KEY}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    restart: unless-stopped

  frontend:
    build: ./ai-invoice-frontend
    ports:
      - "80:80"
    environment:
      - VITE_API_URL=http://backend:3000
    depends_on:
      - backend
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - frontend
      - backend
    restart: unless-stopped
```

### Deploy with Docker

```bash
# Build and start services
docker-compose up -d

# View logs
docker-compose logs -f

# Scale services
docker-compose up -d --scale backend=3
```

---

## 🔧 Production Configuration

### Nginx Configuration

```nginx
# nginx.conf
server {
    listen 80;
    server_name your-domain.com;
    
    # Redirect HTTP to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name your-domain.com;
    
    # SSL Configuration
    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;
    
    # Frontend
    location / {
        proxy_pass http://frontend:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    
    # API
    location /api {
        proxy_pass http://backend:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### SSL Certificate Setup

```bash
# Using Let's Encrypt
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

### Environment Security

```bash
# Use environment-specific configs
# Never commit .env files
echo ".env*" >> .gitignore

# Use secrets management
# AWS Secrets Manager, Azure Key Vault, etc.
```

---

## 📊 Monitoring and Logging

### Application Monitoring

```javascript
// Add to your backend
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
    new winston.transports.Console()
  ]
});
```

### Health Checks

```javascript
// Add health check endpoint
app.get('/health', (req, res) => {
  res.status(200).json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});
```

### Performance Monitoring

```javascript
// Add performance monitoring
const prometheus = require('prom-client');

const httpRequestDuration = new prometheus.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code']
});

// Middleware to collect metrics
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    const duration = (Date.now() - start) / 1000;
    httpRequestDuration
      .labels(req.method, req.route?.path || req.path, res.statusCode)
      .observe(duration);
  });
  next();
});
```

---

## 🔄 CI/CD Pipeline

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test

  deploy-frontend:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: cd ai-invoice-generator-frontend && npm ci
      - run: cd ai-invoice-generator-frontend && npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./ai-invoice-generator-frontend/dist

  deploy-backend:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: "your-app-name"
          heroku_email: "your-email@example.com"
```

---

## 🚨 Troubleshooting

### Common Issues

#### 1. CORS Errors
```javascript
// Ensure CORS is properly configured
app.use(cors({
  origin: process.env.CORS_ORIGIN || 'http://localhost:5173',
  credentials: true
}));
```

#### 2. Database Connection Issues
```javascript
// Check Supabase connection
const { createClient } = require('@supabase/supabase-js');

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
);

// Test connection
supabase.from('users').select('count').then(console.log);
```

#### 3. Environment Variables
```bash
# Check environment variables are loaded
node -e "console.log(process.env.SUPABASE_URL)"
```

#### 4. Port Conflicts
```bash
# Check if port is in use
lsof -i :3000
netstat -tulpn | grep :3000
```

### Performance Optimization

#### 1. Database Indexing
```sql
-- Add indexes for better performance
CREATE INDEX CONCURRENTLY idx_invoices_user_status 
ON invoices(user_id, status);

CREATE INDEX CONCURRENTLY idx_invoices_date_range 
ON invoices(invoice_date, due_date);
```

#### 2. Caching
```javascript
// Add Redis caching
const redis = require('redis');
const client = redis.createClient();

// Cache frequently accessed data
const cacheKey = `user:${userId}:invoices`;
const cached = await client.get(cacheKey);
if (cached) return JSON.parse(cached);
```

#### 3. Rate Limiting
```javascript
// Add rate limiting
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api', limiter);
```

---

## 📈 Scaling Considerations

### Horizontal Scaling

1. **Load Balancer**: Use nginx or cloud load balancer
2. **Multiple Instances**: Run multiple backend instances
3. **Database Scaling**: Use read replicas for read-heavy operations
4. **CDN**: Use CloudFront or similar for static assets

### Vertical Scaling

1. **Increase Resources**: More CPU/RAM for instances
2. **Database Optimization**: Better indexing and query optimization
3. **Caching**: Redis for session and data caching
4. **Connection Pooling**: Optimize database connections

---

## 🔒 Security Checklist

### Production Security

- [ ] HTTPS enabled with valid SSL certificate
- [ ] Environment variables secured
- [ ] Database RLS policies configured
- [ ] Rate limiting implemented
- [ ] Input validation and sanitization
- [ ] CORS properly configured
- [ ] Security headers set
- [ ] Regular security updates
- [ ] Monitoring and logging enabled
- [ ] Backup strategy implemented

### Security Headers

```javascript
// Add security headers
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
}));
```

This comprehensive deployment guide ensures your AI Invoice Generator application is deployed securely and efficiently across various platforms and environments.
