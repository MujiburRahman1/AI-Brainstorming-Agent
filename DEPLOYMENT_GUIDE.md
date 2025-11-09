# 🚀 Deployment Guide - AI Brainstorming Agent

یہ guide مختلف platforms پر deployment کے steps بتاتی ہے.

## 📋 Deployment Options

### Option 1: **Vercel (Frontend) + Railway/Render (Backend)** ⭐ Recommended (Easiest & Free)

یہ سب سے آسان اور free option ہے beginners کے لیے.

#### Frontend on Vercel:
1. Vercel account بنائیں: https://vercel.com
2. GitHub repository connect کریں
3. Auto-deployment ہو جائے گا

#### Backend on Railway/Render:
- **Railway**: https://railway.app (Free tier available)
- **Render**: https://render.com (Free tier available)

---

### Option 2: **Google Cloud Run** (Original Plan)

Production-ready deployment for both frontend and backend.

#### Requirements:
- Google Cloud Account
- Billing enabled
- gcloud CLI installed

---

### Option 3: **Netlify (Frontend) + Fly.io (Backend)**

Another free option with good performance.

---

## 🎯 Step-by-Step: Option 1 (Vercel + Railway) - Recommended

### Part A: Deploy Frontend to Vercel

#### Step 1: Prepare Frontend for Production
```bash
cd frontend
npm run build
```

#### Step 2: Deploy to Vercel
1. Go to https://vercel.com
2. Sign up/Login with GitHub
3. Click "Add New Project"
4. Import your repository: `MujiburRahman1/AI-Brainstorming-Agent`
5. Configure:
   - **Framework Preset**: Vite
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`

#### Step 3: Environment Variables
Add these in Vercel Dashboard:
```
VITE_BACKEND_URL=https://your-backend-url.railway.app
VITE_ELEVENLABS_API_KEY=your_key (optional)
VITE_ELEVENLABS_VOICE_ID=21m00Tcm4TlvDq8ikWAM
```

### Part B: Deploy Backend to Railway

#### Step 1: Prepare Backend
```bash
cd backend
# Make sure requirements.txt is updated
```

#### Step 2: Create Railway Account
1. Go to https://railway.app
2. Sign up with GitHub
3. Click "New Project"
4. Select "Deploy from GitHub repo"
5. Select your repository

#### Step 3: Configure Railway
- **Root Directory**: `backend`
- **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- **Environment**: Python 3.11

#### Step 4: Set Environment Variables
In Railway Dashboard:
```
PORT=8000
APP_ENV=production
```

#### Step 5: Get Backend URL
- Railway automatically provides a URL
- Copy this URL and update Vercel's `VITE_BACKEND_URL`

---

## 🎯 Step-by-Step: Option 2 (Google Cloud Run)

### Prerequisites
```bash
# Install gcloud CLI
# https://cloud.google.com/sdk/docs/install

# Login to Google Cloud
gcloud auth login

# Set project
gcloud config set project YOUR_PROJECT_ID

# Enable APIs
gcloud services enable run.googleapis.com
gcloud services enable cloudbuild.googleapis.com
gcloud services enable artifactregistry.googleapis.com
```

### Deploy Backend to Cloud Run

#### Step 1: Build and Push Docker Image
```bash
# Set variables
export PROJECT_ID=your-project-id
export REGION=us-east5
export SERVICE_NAME=brainstorming-backend

# Build and push
gcloud builds submit --tag gcr.io/$PROJECT_ID/$SERVICE_NAME

# Deploy to Cloud Run
gcloud run deploy $SERVICE_NAME \
  --image gcr.io/$PROJECT_ID/$SERVICE_NAME \
  --platform managed \
  --region $REGION \
  --allow-unauthenticated \
  --memory 1Gi \
  --port 8080
```

#### Step 2: Deploy Frontend to Cloud Run
```bash
# Build frontend first
cd frontend
npm run build

# Create a simple server for frontend
# (We'll need to create a Dockerfile for frontend)
```

---

## 🎯 Step-by-Step: Option 3 (Netlify + Fly.io)

### Frontend on Netlify
1. Go to https://netlify.com
2. Connect GitHub repository
3. Set build settings:
   - Base directory: `frontend`
   - Build command: `npm run build`
   - Publish directory: `frontend/dist`

### Backend on Fly.io
1. Install Fly CLI: https://fly.io/docs/getting-started/installing-flyctl/
2. Login: `fly auth login`
3. Create app: `fly launch`
4. Deploy: `fly deploy`

---

## 🔧 Quick Deploy Scripts

### For Vercel (Frontend)
Create `vercel.json` in frontend directory:
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite"
}
```

### For Railway (Backend)
Create `Procfile` in backend directory:
```
web: uvicorn main:app --host 0.0.0.0 --port $PORT
```

---

## 🌐 Environment Variables

### Frontend (.env.production)
```
VITE_BACKEND_URL=https://your-backend-url.com
VITE_ELEVENLABS_API_KEY=your_key
VITE_ELEVENLABS_VOICE_ID=21m00Tcm4TlvDq8ikWAM
```

### Backend
```
APP_ENV=production
PORT=8000
GCP_PROJECT_ID=your-project-id (if using GCP)
USE_VERTEX=false
```

---

## ✅ Post-Deployment Checklist

- [ ] Backend is accessible (check `/health` endpoint)
- [ ] Frontend can connect to backend
- [ ] CORS is configured correctly
- [ ] Environment variables are set
- [ ] SSL/HTTPS is enabled
- [ ] Domain is configured (optional)

---

## 🐛 Troubleshooting

### CORS Errors
Make sure backend CORS allows your frontend domain:
```python
# In backend/main.py
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://your-frontend.vercel.app"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Backend Connection Issues
- Check backend URL is correct
- Verify backend is running
- Check firewall/security settings

### Build Failures
- Check Node.js version (18+)
- Check Python version (3.11+)
- Verify all dependencies are in package.json/requirements.txt

---

## 📊 Cost Comparison

| Platform | Free Tier | Paid Tier |
|----------|-----------|-----------|
| Vercel | ✅ Yes (100GB bandwidth) | $20/month |
| Railway | ✅ Yes (500 hours/month) | $5/month |
| Render | ✅ Yes (750 hours/month) | $7/month |
| Google Cloud Run | ✅ Yes (2M requests) | Pay as you go |
| Netlify | ✅ Yes (100GB bandwidth) | $19/month |
| Fly.io | ✅ Yes (3 shared VMs) | Pay as you go |

---

## 🎯 Recommended Setup for Beginners

**Frontend**: Vercel (Free, Easy, Auto-deploy from GitHub)
**Backend**: Railway (Free tier, Simple setup)

یہ setup:
- ✅ Completely free
- ✅ Easy to set up
- ✅ Auto-deploys on git push
- ✅ Good performance
- ✅ SSL/HTTPS included

---

## 📝 Next Steps

1. Choose your deployment option
2. Follow the step-by-step guide
3. Test the deployed application
4. Update environment variables
5. Share your deployed URL! 🎉

---

## 🆘 Need Help?

اگر آپ کو deployment میں کوئی problem آئے تو:
1. Check the troubleshooting section
2. Verify all environment variables
3. Check server logs
4. Test endpoints manually

Happy Deploying! 🚀

