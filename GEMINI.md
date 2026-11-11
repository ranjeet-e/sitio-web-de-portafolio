# Production Architecture Guidelines

## 1. Never Use Rewrites in `vercel.json`
- **Do NOT** use `"rewrites"` to reverse-proxy backend traffic (e.g., `/api/:path*`).
- Every single proxied request drains Vercel's 1M Edge Request limit and causes complete account lockouts (`HTTP 402 DEPLOYMENT_DISABLED`).
- **Standard**: Have the frontend call the backend directly using environment variables (e.g. `VITE_API_BASE_URL=https://api.yourdomain.com` or `https://your-backend.onrender.com`), and allow the frontend domain in backend CORS.

## 2. Never Host Production Backends on Local / College Intranet IPs
- **Do NOT** point production traffic to campus/home IPs (e.g., `121.x.x.x:5050` or `172.x.x.x`).
- Enterprise hardware firewalls (like Fortinet FortiGate) block custom ports and newly registered domains, and campus power/Wi-Fi resets will kill the app worldwide.
- **Standard**: Always deploy backends to standard cloud platforms (Render, Railway, Koyeb, Fly.io) on port 443 with HTTPS.

## 3. Prevent Free Tier Cold Starts
- Free cloud backends (like Render) sleep after 15 minutes of inactivity.
- **Standard**: Set up an automated 10-minute ping using a free uptime monitor (UptimeRobot, Cron-job.org, or Cloudflare Cron Triggers) to keep the backend warm 24/7.

## 4. Prefer Cloudflare for Production Frontends
- For high-traffic or public college apps, deploy static assets to Cloudflare Workers / Pages instead of Vercel. Cloudflare provides unlimited bandwidth and zero edge request caps on the free tier.
