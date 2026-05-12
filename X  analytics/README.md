# XPulse Backend

Express.js proxy server that solves the CORS problem by making all X API calls server-side, then serving the results to the frontend.

## Why a backend?
Browsers block direct calls to `api.twitter.com` from web pages (CORS policy). This backend sits in between — your frontend calls `localhost:3000`, the server calls X's API with your Bearer Token, and returns the data safely.

## Setup

### 1. Install Node.js
Download from https://nodejs.org (v18 or higher recommended)

### 2. Install dependencies
```bash
cd xpulse-backend
npm install
```

### 3. Add your Bearer Token
```bash
cp .env.example .env
```
Open `.env` and paste your Bearer Token:
```
BEARER_TOKEN=AAAAAAAAAAAAAAAAAAAAADi%2B...
```
Get your token at: https://developer.twitter.com/en/portal/dashboard

### 4. Run the server
```bash
npm start
```
You should see:
```
✅  XPulse backend running at http://localhost:3000
```

### 5. Open the app
Go to http://localhost:3000 in your browser. Type any username and click Analyze.

---

## API Endpoints

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/health` | Check server + token validity |
| GET | `/api/user/:username` | Profile data for a username |
| GET | `/api/user/:username/tweets` | Recent tweets with metrics |
| GET | `/api/analytics/:username` | Full analytics (profile + tweets + computed KPIs) |
| GET | `/api/search?q=query` | Search recent tweets |

### Example responses

**GET /api/analytics/jack**
```json
{
  "success": true,
  "user": {
    "id": "12",
    "name": "jack",
    "username": "jack",
    "public_metrics": {
      "followers_count": 6700000,
      "following_count": 4000,
      "tweet_count": 28000
    }
  },
  "tweets": [...],
  "analytics": {
    "totalLikes": 84200,
    "totalRetweets": 12400,
    "totalReplies": 8100,
    "totalEngagements": 104700,
    "estimatedImpressions": 4820000,
    "engagementRate": 1.56,
    "breakdown": { "likes": 80, "retweets": 12, "replies": 8 },
    "topTweets": [...],
    "bestHour": 14
  }
}
```

---

## Development (auto-restart on file changes)
```bash
npm run dev
```

## Deploy to the web (optional)
You can deploy this backend to Railway, Render, or Fly.io — all have free tiers. Set `BEARER_TOKEN` as an environment variable in their dashboard, and update the `API` constant in `public/index.html` to your deployed URL.

---

## File structure
```
xpulse-backend/
├── server.js          ← Express backend (all X API calls happen here)
├── package.json
├── .env               ← Your Bearer Token (DO NOT commit this)
├── .env.example       ← Template for .env
├── .gitignore
└── public/
    └── index.html     ← Frontend (served by Express)
```
