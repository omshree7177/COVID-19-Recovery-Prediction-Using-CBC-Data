# Deployment Guide

Complete guide for deploying the COVID-19 Patient Outcome Prediction System to various platforms.

## Pre-deployment Checklist

- [ ] All tests pass locally
- [ ] Code is peer-reviewed
- [ ] Documentation is updated
- [ ] Environment variables are configured
- [ ] Database/external services are set up
- [ ] Error tracking is enabled
- [ ] Monitoring is configured
- [ ] Backup strategy is in place

## Deployment Options

## Option 1: Vercel (Recommended)

### Advantages
- Zero-config deployment
- Automatic HTTPS
- Global CDN
- Built-in analytics
- Simple rollback

### Steps

1. **Push to GitHub**
   ```bash
   git push origin main
   ```

2. **Connect to Vercel**
   - Go to https://vercel.com
   - Click "Import Project"
   - Select your GitHub repository
   - Click "Import"

3. **Configure Environment Variables**
   - Go to Settings → Environment Variables
   - Add production variables:
     ```
     PYTHON_PATH=/usr/bin/python3
     ML_ARTIFACTS_PATH=/root/ml_artifacts
     NEXT_PUBLIC_APP_NAME=COVID-19 Predictor
     ```

4. **Deploy**
   - Click "Deploy"
   - Wait for build to complete

5. **Verify**
   - Visit your Vercel URL
   - Test prediction functionality

### Post-deployment
- Configure custom domain (optional)
- Set up analytics
- Configure error tracking

---

## Option 2: Docker (Self-hosted)

### Advantages
- Full control
- No vendor lock-in
- Can run anywhere
- Predictable environment

### Prerequisites
- Docker and Docker Compose installed
- Linux server (Ubuntu 20.04+ recommended)
- Domain name (optional)
- SSL certificate (recommended)

### Deployment

1. **Prepare Server**
   ```bash
   # Update system
   sudo apt update && sudo apt upgrade -y
   
   # Install Docker
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   
   # Install Docker Compose
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   ```

2. **Clone Repository**
   ```bash
   git clone https://github.com/your-org/covid-prediction.git
   cd covid-prediction
   ```

3. **Create .env.production**
   ```bash
   cp .env.example .env.production
   # Edit .env.production with production values
   nano .env.production
   ```

4. **Build and Start**
   ```bash
   docker-compose up -d --build
   ```

5. **Verify**
   ```bash
   docker-compose logs -f app
   curl http://localhost:3000
   ```

### Nginx Reverse Proxy

Create `/etc/nginx/sites-available/covid-predictor`:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable site:
```bash
sudo ln -s /etc/nginx/sites-available/covid-predictor /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### SSL Certificate (Let's Encrypt)

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

### Container Management

```bash
# View logs
docker-compose logs -f app

# Restart
docker-compose restart

# Stop
docker-compose down

# Update
git pull
docker-compose up -d --build
```

---

## Option 3: AWS (EC2/ECS)

### EC2 Deployment

1. **Launch EC2 Instance**
   - Ubuntu 20.04 LTS
   - t3.medium or larger
   - Security group: Allow 80, 443

2. **SSH and Setup**
   ```bash
   ssh -i your-key.pem ubuntu@your-instance-ip
   
   # Follow Docker setup from Option 2
   ```

### ECS Deployment

1. **Create ECR Repository**
   ```bash
   aws ecr create-repository --repository-name covid-predictor
   ```

2. **Push Image**
   ```bash
   docker build -t covid-predictor .
   docker tag covid-predictor:latest YOUR_AWS_ACCOUNT.dkr.ecr.YOUR_REGION.amazonaws.com/covid-predictor:latest
   docker push YOUR_AWS_ACCOUNT.dkr.ecr.YOUR_REGION.amazonaws.com/covid-predictor:latest
   ```

3. **Create ECS Task**
   - Define task definition with Docker image
   - Configure port 3000
   - Set environment variables

4. **Launch Service**
   - Create service in ECS cluster
   - Configure load balancer
   - Set auto-scaling policies

---

## Option 4: Google Cloud Run

### Steps

1. **Build Image**
   ```bash
   gcloud builds submit --tag gcr.io/PROJECT_ID/covid-predictor
   ```

2. **Deploy**
   ```bash
   gcloud run deploy covid-predictor \
     --image gcr.io/PROJECT_ID/covid-predictor \
     --platform managed \
     --region us-central1 \
     --memory 2Gi \
     --cpu 2
   ```

3. **Configure Environment Variables**
   ```bash
   gcloud run services update covid-predictor \
     --set-env-vars PYTHON_PATH=/usr/bin/python3
   ```

---

## Option 5: DigitalOcean App Platform

1. **Connect Repository**
   - Go to DigitalOcean dashboard
   - Click "Create App"
   - Select GitHub repository

2. **Configure**
   - Set build command: `pnpm build`
   - Set run command: `pnpm start`
   - Add environment variables

3. **Deploy**
   - Click "Deploy App"
   - Wait for deployment

---

## Post-deployment Tasks

### Monitoring

1. **Set up Error Tracking**
   - Sentry: https://sentry.io
   - Papertrail: https://papertrailapp.com

2. **Configure Logging**
   - CloudWatch (AWS)
   - Stackdriver (GCP)
   - Container logs

3. **Performance Monitoring**
   - Vercel Analytics
   - DataDog
   - New Relic

### Backup Strategy

```bash
# Backup ML artifacts
tar -czf ml_artifacts_backup.tar.gz /root/ml_artifacts/

# Backup database (if using one)
pg_dump database_name > backup.sql
```

### Security

- [ ] Enable HTTPS/SSL
- [ ] Configure firewall rules
- [ ] Set up DDoS protection
- [ ] Enable authentication if needed
- [ ] Regular security updates

### Performance Optimization

- [ ] Enable caching headers
- [ ] Compress responses
- [ ] Optimize images
- [ ] Use CDN

### Maintenance

- [ ] Set up automated backups
- [ ] Schedule regular updates
- [ ] Monitor resource usage
- [ ] Keep dependencies updated

## Rollback Procedure

### Vercel
```bash
# Redeploy previous version
vercel rollback
```

### Docker
```bash
# Roll back to previous image
docker-compose down
git checkout PREVIOUS_COMMIT
docker-compose up -d --build
```

### AWS EC2
```bash
# Restore from snapshot or redeploy
```

## Troubleshooting

### Application won't start
- Check environment variables
- Verify Python is installed
- Check port availability
- Review application logs

### Prediction endpoint fails
- Verify ML artifacts exist
- Check Python dependencies
- Review API logs
- Test locally

### Performance issues
- Monitor CPU/memory usage
- Check database queries
- Optimize images
- Use caching

## Support

- Documentation: `/documentation`
- GitHub Issues: Report problems
- Email: support@example.com

---

For more help, see QUICKSTART.md and README.md
