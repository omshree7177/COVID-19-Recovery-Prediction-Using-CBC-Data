# Quick Start Guide

## Installation & Setup

### Option 1: Local Development

**Prerequisites:**
- Node.js 18+ and npm/pnpm
- Python 3.8+
- Git

**Steps:**

```bash
# 1. Clone the repository
git clone <your-repo-url>
cd covid-prediction-system

# 2. Install Node dependencies
pnpm install

# 3. Train the ML model
python scripts/train_model.py

# 4. Start development server
pnpm dev

# 5. Open browser
# Visit http://localhost:3000
```

### Option 2: Docker

**Prerequisites:**
- Docker
- Docker Compose

**Steps:**

```bash
# 1. Build and start containers
docker-compose up --build

# 2. Open browser
# Visit http://localhost:3000
```

### Option 3: Vercel Deployment

**Steps:**

```bash
# 1. Install Vercel CLI
npm install -g vercel

# 2. Login to Vercel
vercel login

# 3. Deploy
vercel deploy
```

## First Time Usage

### Step 1: Navigate to Prediction Page
- Open http://localhost:3000 (or your deployed URL)
- Click "Predict" or "Go to Predictor"

### Step 2: Enter Patient Data
Fill in the Complete Blood Count measurements:
- WBC, RBC, Hemoglobin, Hematocrit
- MCV, Platelet, MPV
- Neutrophil, Lymphocyte, Monocyte, Eosinophil, Basophil
- ESR

**Example values (normal range):**
```
WBC: 7.5 (K/uL)
RBC: 4.8 (M/uL)
Hemoglobin: 14.2 (g/dL)
Hematocrit: 42.5 (%)
MCV: 88 (fL)
Platelet: 250 (K/uL)
MPV: 8.5 (fL)
Neutrophil: 65 (%)
Lymphocyte: 25 (%)
Monocyte: 7 (%)
Eosinophil: 2 (%)
Basophil: 1 (%)
ESR: 20 (mm/hr)
```

### Step 3: Make Prediction
- Click "Predict Outcome"
- View results with confidence score

### Step 4: Explore Analytics
- Navigate to "Analytics" tab
- View prediction statistics and trends

## Configuration

### Environment Variables

Create `.env.local` file:

```env
NODE_ENV=development
NEXT_PUBLIC_API_URL=http://localhost:3000
PYTHON_PATH=/usr/bin/python3
ML_ARTIFACTS_PATH=/root/ml_artifacts
```

### ML Model Paths

The system expects:
- Model: `/root/ml_artifacts/best_model.joblib`
- Scaler: `/root/ml_artifacts/scaler.joblib`
- Metadata: `/root/ml_artifacts/model_metadata.json`
- Stats: `/root/ml_artifacts/feature_stats.json`

If training a new model:
```bash
python scripts/train_model.py
```

## Common Issues

### Issue: "CSV file not found"
**Solution:** Ensure the data file is at `/vercel/share/v0-project/public/data/COVID-19_CBC_Data.csv`

### Issue: "Python module not found"
**Solution:** Install required packages:
```bash
pip install scikit-learn joblib numpy pandas
```

### Issue: "API endpoint not responding"
**Solution:** 
1. Check that Python is available: `which python3`
2. Verify scripts are executable: `chmod +x scripts/*.py`
3. Check server logs: `pnpm dev` shows detailed errors

### Issue: Port 3000 already in use
**Solution:** Use a different port:
```bash
pnpm dev -p 3001
```

## Development Tips

### Hot Reload
- Frontend changes auto-reload in development
- Edit `.tsx` files in `/app` or `/components`

### API Testing
Use the built-in API endpoint:
```bash
curl -X POST http://localhost:3000/api/predict \
  -H "Content-Type: application/json" \
  -d '{"features": {"WBC": 7.5, "RBC": 4.8, ...}}'
```

### Database Connection
For production, connect a database in the API routes.

## Production Deployment

### Pre-deployment Checklist
- [ ] Update `.env.production` with correct values
- [ ] Run `pnpm build` to verify build succeeds
- [ ] Test with `pnpm start`
- [ ] Update documentation with your deployment URL
- [ ] Configure error tracking (e.g., Sentry)
- [ ] Set up monitoring and alerts

### Vercel
1. Connect GitHub repository
2. Configure environment variables
3. Deploy automatically on push

### Self-hosted
1. Use Docker image
2. Set environment variables
3. Configure reverse proxy (nginx)
4. Set up HTTPS certificates

## Support & Documentation

- **Full Documentation:** Visit `/documentation` page
- **API Reference:** See `/api/predict` endpoint
- **GitHub Issues:** Report bugs or request features
- **README:** See project root README.md

## Next Steps

1. ✅ Set up local development
2. ✅ Make your first prediction
3. ✅ Explore the analytics dashboard
4. ✅ Read the technical documentation
5. ✅ Deploy to your platform
6. ✅ Integrate with your clinical system (optional)

Happy predicting!
