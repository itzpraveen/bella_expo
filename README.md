# Expo Coupon Distribution System

A mobile-responsive web application for digitizing coupon distribution at expos and trade shows. Staff can enter customer details and instantly send discount coupons via WhatsApp.

![Expo Coupon System](https://img.shields.io/badge/Status-Production%20Ready-green)

## Features

- **Staff Authentication**: Secure login system with role-based access
- **Mobile-First Design**: Optimized for use on phones at expo stalls
- **Real-time Coupon Generation**: Unique codes generated instantly
- **WhatsApp Integration**: Automatic coupon delivery via Twilio
- **Admin Dashboard**: View all data, filter, search, and export to Excel
- **Staff Management**: Add/manage staff accounts with activity tracking
- **Offline Resilient**: Stores data even if WhatsApp fails temporarily

## Quick Start

### Prerequisites

- Node.js 18+ 
- npm or yarn

### Installation

```bash
# Clone/download the project
cd expo-coupon-app

# Setup Backend
cd backend
npm install
cp .env.example .env
# Edit .env with your settings

# Setup Frontend
cd ../frontend
npm install
```

### Running Locally

```bash
# Terminal 1 - Backend
cd backend
npm run dev
# Server runs on http://localhost:3001

# Terminal 2 - Frontend
cd frontend
npm run dev
# App runs on http://localhost:3000
```

### Default Login

- **Username**: `admin`
- **Password**: `admin123`

⚠️ **Change this immediately in production!**

## Configuration

### Environment Variables (backend/.env)

```env
# Server
PORT=3001
NODE_ENV=production

# IMPORTANT: Change this to a random string!
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production

# Twilio WhatsApp (see setup below)
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_WHATSAPP_NUMBER=+14155238886

# Customize branches for your business
BRANCHES=Calicut,Kochi,Thrissur,Kannur,Malappuram
```

## WhatsApp Setup (Twilio)

### Option 1: Twilio Sandbox (Free - Good for Testing)

1. Create account at [twilio.com](https://www.twilio.com)
2. Go to **Messaging → Try it out → Send a WhatsApp message**
3. Note the sandbox number (usually +1 415 523 8886)
4. **Important**: Customers must first send "join <your-sandbox-word>" to the sandbox number

**Limitation**: Each customer must opt-in by messaging the sandbox first.

### Option 2: WhatsApp Business API (Recommended for Production)

1. Apply for WhatsApp Business API access via Twilio
2. Get your business number verified
3. Create a message template for promotional messages:
   ```
   Template Name: expo_coupon
   Message: Welcome! 🎉 Here is your 15% discount coupon code: {{1}}. Valid for 4 months at our showroom. Thank you for visiting!
   ```
4. Use the approved business number in your config

**For your February 1st expo**: If you can't get Business API approved in time, use the Sandbox but display a QR code at your stall asking customers to first message your sandbox number.

## Deployment Options

### Option 1: Railway (Recommended - Easiest)

1. Push code to GitHub
2. Go to [railway.app](https://railway.app)
3. Create new project → Deploy from GitHub
4. Add environment variables
5. Deploy!

**Estimated cost**: ~$5/month

### Option 2: Render

1. Create account at [render.com](https://render.com)
2. Create Web Service for backend
3. Create Static Site for frontend
4. Add environment variables

**Free tier available** with limitations

### Option 3: DigitalOcean App Platform

1. Go to [digitalocean.com](https://digitalocean.com)
2. Create App → Choose GitHub repo
3. Configure services
4. Deploy

**$5/month basic plan**

### Option 4: VPS (Full Control)

```bash
# On your VPS (Ubuntu)
sudo apt update
sudo apt install nodejs npm nginx certbot

# Clone and setup
git clone <your-repo>
cd expo-coupon-app

# Backend
cd backend
npm install
npm run build

# Frontend
cd ../frontend
npm install
npm run build

# Use PM2 for process management
npm install -g pm2
pm2 start backend/server.js --name coupon-api

# Copy frontend/dist to nginx webroot
sudo cp -r frontend/dist/* /var/www/html/
```

## Production Checklist

- [ ] Change default admin password
- [ ] Set strong JWT_SECRET (use `openssl rand -hex 32`)
- [ ] Configure Twilio credentials
- [ ] Update BRANCHES list for your business
- [ ] Set FRONTEND_URL for CORS
- [ ] Enable HTTPS
- [ ] Set up database backups
- [ ] Test WhatsApp delivery

## API Endpoints

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | /api/auth/login | Staff login | No |
| GET | /api/auth/me | Get current user | Yes |
| GET | /api/branches | Get branch list | Yes |
| POST | /api/coupons | Create coupon | Yes |
| GET | /api/coupons | List coupons | Yes |
| GET | /api/coupons/export | Export Excel | Admin |
| POST | /api/coupons/:id/resend | Resend WhatsApp | Yes |
| GET | /api/stats | Get statistics | Yes |
| GET | /api/staff | List staff | Admin |
| POST | /api/staff | Add staff | Admin |
| PATCH | /api/staff/:id/toggle | Toggle status | Admin |

## Troubleshooting

### WhatsApp not sending?

1. Check Twilio credentials in .env
2. Verify phone number format (10 digits for India)
3. For Sandbox: Ensure customer has opted-in
4. Check Twilio console for error logs

### Login not working?

1. Ensure backend is running
2. Check browser console for errors
3. Verify CORS settings (FRONTEND_URL)

### Database issues?

The SQLite database is created automatically at `backend/data/coupon.db`. If corrupted:
```bash
rm backend/data/coupon.db
npm start  # Will recreate with fresh schema
```

## Support

For issues specific to:
- **Twilio**: [twilio.com/docs](https://www.twilio.com/docs)
- **Deployment**: Check platform-specific documentation

## License

MIT License - Free for commercial use

---

Built for Majestic Jewellers Expo - February 2025
