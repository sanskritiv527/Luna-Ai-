# Luna AI - Deployment Guide

Luna AI is a full-stack menstrual cycle coaching application with a Next.js frontend and Python ML backend.

## Architecture

- **Frontend**: Next.js 16+ with React, TypeScript
- **Backend**: Python Flask with Gemini AI and ML models
- **Database**: Supabase (PostgreSQL)
- **AI Models**: Google Gemini for natural language, custom ML for cycle prediction

## Local Development

### Prerequisites
- Node.js 18+
- Python 3.9+
- Supabase account
- Google Gemini API key

### Setup Frontend

\`\`\`bash
# Install dependencies
npm install

# Create .env.local with your keys
cp .env.example .env.local

# Run Next.js development server
npm run dev
\`\`\`

Frontend runs on `http://localhost:3000`

### Setup Backend

\`\`\`bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env with your keys
cp .env.example .env

# Run Flask server
python app.py
\`\`\`

Backend runs on `http://localhost:5000`

## Deployment

### Deploy Frontend to Vercel

\`\`\`bash
# Push to GitHub
git push origin main

# Connect to Vercel from https://vercel.com
# Select your repository and deploy
\`\`\`

Environment variables on Vercel:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `NEXT_PUBLIC_DEV_SUPABASE_REDIRECT_URL` (set to production URL)
- `PYTHON_BACKEND_URL` (your backend URL)

### Deploy Backend to Render or Railway

#### Using Render

1. Connect GitHub repository to Render
2. Create new Web Service
3. Build command: `pip install -r requirements.txt`
4. Start command: `gunicorn --bind 0.0.0.0:5000 app:app`
5. Set environment variables:
   - `GOOGLE_GENERATIVE_AI_API_KEY`
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `SUPABASE_SERVICE_ROLE_KEY`
   - `FLASK_ENV=production`

#### Using Railway

1. Connect GitHub repository to Railway
2. Add environment variables
3. Detect Python and deploy

#### Using Docker

\`\`\`bash
cd backend
docker build -t luna-ai-backend .
docker run -p 5000:5000 --env-file .env luna-ai-backend
\`\`\`

## Database Setup

Run the SQL migration script in Supabase:

1. Go to Supabase dashboard
2. Navigate to SQL Editor
3. Run `/scripts/001_create_tables.sql` or use the `/setup` endpoint

Or use the setup endpoint:
\`\`\`bash
curl -X GET http://localhost:3000/setup
\`\`\`

## API Endpoints

### Frontend Routes
- `/` - Landing page
- `/auth/sign-up` - User registration
- `/auth/login` - User login
- `/dashboard` - User dashboard
- `/coach/[type]` - Coach pages (daily-coach, deeper-dive, doc-qna)
- `/settings` - User profile and cycle tracking

### Backend Routes
- `GET /health` - Health check
- `POST /api/coach/daily` - Daily Coach
- `POST /api/coach/deeper-dive` - Deeper Dive Coach
- `POST /api/coach/doc-qna` - Doc QNA Coach
- `POST /api/cycle/predict` - ML cycle prediction
- `POST /api/cycle/analyze` - Cycle pattern analysis

## Environment Variables

### Required for Both
- `NEXT_PUBLIC_SUPABASE_URL` - Supabase project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Supabase public key
- `GOOGLE_GENERATIVE_AI_API_KEY` - Google Gemini API key

### Backend Only
- `SUPABASE_SERVICE_ROLE_KEY` - Supabase service key (for backend)
- `FLASK_ENV` - Set to 'production' for production

### Frontend Only
- `PYTHON_BACKEND_URL` - URL to Python backend
- `NEXT_PUBLIC_DEV_SUPABASE_REDIRECT_URL` - Local redirect URL for auth

## Monitoring

### Frontend
- Use Vercel Analytics for performance monitoring
- Check Next.js error logs in Vercel dashboard

### Backend
- Monitor Flask logs on Render/Railway
- Use database query performance metrics in Supabase dashboard

## Troubleshooting

### Backend not responding
- Ensure `PYTHON_BACKEND_URL` is correct in frontend env vars
- Check backend logs on deployment platform
- Verify all environment variables are set on backend

### Database connection issues
- Verify Supabase credentials are correct
- Check Row Level Security policies in Supabase dashboard
- Run setup endpoint if tables are missing

### Gemini API errors
- Verify API key is valid and has appropriate quota
- Check Google Cloud Console for billing

## Support

For issues or questions:
1. Check logs on your deployment platform
2. Verify all environment variables
3. Test locally first before deploying
4. Check Supabase dashboard for database issues
\`\`\`
