# Quick Deployment Guide

## Fastest Option: Railway (5 minutes)

### Step 1: Prepare Your Code
```bash
# Create a GitHub repository and push the code
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR_USERNAME/expo-coupon-app.git
git push -u origin main
```

### Step 2: Deploy on Railway
1. Go to [railway.app](https://railway.app) and sign in with GitHub
2. Click **"New Project"** → **"Deploy from GitHub repo"**
3. Select your `expo-coupon-app` repository
4. Railway will auto-detect and deploy

### Step 3: Add Environment Variables
In Railway dashboard, go to your project → **Variables** tab → Add these:

```
JWT_SECRET=<generate-a-random-32-character-string>
TWILIO_ACCOUNT_SID=<your-twilio-sid>
TWILIO_AUTH_TOKEN=<your-twilio-token>
TWILIO_WHATSAPP_NUMBER=+14155238886
BRANCHES=Calicut,Kochi,Thrissur,Kannur,Malappuram
```

### Step 4: Generate Domain
- Go to **Settings** → **Networking** → **Generate Domain**
- You'll get a URL like: `expo-coupon-app-production.up.railway.app`

---

## Alternative: Render (Free Tier Available)

### Step 1: Create render.yaml
Add this file to your project root:

```yaml
services:
  - type: web
    name: expo-coupon
    env: node
    buildCommand: cd backend && npm install
    startCommand: cd backend && node server.production.js
    envVars:
      - key: JWT_SECRET
        generateValue: true
      - key: NODE_ENV
        value: production
```

### Step 2: Deploy
1. Go to [render.com](https://render.com) → **New** → **Blueprint**
2. Connect your GitHub repo
3. Add environment variables
4. Deploy!

---

## Test Before Expo Day

1. **Login Test**: Open the app URL, login with admin/admin123
2. **Create Staff**: Add your 4-5 staff members
3. **Test Coupon**: Create a test coupon, verify WhatsApp delivery
4. **Export Test**: Download Excel to verify data format

## Day-of Checklist

- [ ] All staff have login credentials
- [ ] Phone batteries charged (for staff devices)
- [ ] Test coupon sent successfully
- [ ] Branch names match your stalls
- [ ] Admin dashboard accessible for monitoring

## Twilio Sandbox Quick Setup

If you haven't set up WhatsApp Business API:

1. Go to [console.twilio.com](https://console.twilio.com)
2. Navigate to **Messaging** → **Try it out** → **WhatsApp**
3. Note the join code (e.g., "join example-word")
4. **At your expo stall**: Display a sign asking customers to first message:
   ```
   "To receive your discount coupon instantly via WhatsApp,
   please message 'join example-word' to +1 415 523 8886"
   ```

---

## Support Contacts

- Railway Support: [railway.app/help](https://railway.app/help)
- Twilio Support: [twilio.com/help](https://www.twilio.com/help)
