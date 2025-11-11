# 🚌 Public Transport Real-Time Tracking System - Complete Setup Guide

## 📋 Project Overview

A real-time public transport tracking system with:
- **Backend**: Node.js + Express + MongoDB + Socket.IO
- **Driver App**: React Native (Expo) for drivers to track trips and location
- **Passenger App**: React Native (Expo) for passengers to view all active buses on map

---

## 🔧 Prerequisites

Before starting, ensure you have:
- **Node.js** (v16+): [Download](https://nodejs.org/)
- **npm** (v8+): Comes with Node.js
- **Git**: [Download](https://git-scm.com/)
- **Expo CLI**: Install with `npm install -g expo-cli`
- **Expo Go App**: Install on Android device from [Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent)
- **MongoDB Atlas Account**: [Create free account](https://www.mongodb.com/cloud/atlas)

---

## 🚀 Step 1: Setup Backend Server

### 1.1 Create MongoDB Database

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a new cluster
4. Create database user with credentials
5. Get connection string: `mongodb+srv://username:password@cluster.mongodb.net/database-name`

### 1.2 Configure Backend

```bash
cd "d:\public transport\backend"

# Copy .env file (if exists) or create new
# Edit .env with your MongoDB connection string
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/public-transport
# JWT_SECRET=your-secret-key-here
```

### 1.3 Install Dependencies & Start Backend

```bash
cd "d:\public transport\backend"
npm install
npm start
```

**Expected Output:**
```
✅ Connected to MongoDB
🚀 Server running on port 3000
📡 WebSocket server ready
```

**Backend URL**: `http://192.168.31.97:3000`

---

## 📱 Step 2: Setup Driver App

### 2.1 Install Dependencies

```bash
cd "d:\public transport\driver-app"
npm install
```

### 2.2 Start Driver App

```bash
cd "d:\public transport\driver-app"
npx expo start --tunnel --port 8082
```

**Expected Output:**
- QR Code displayed
- Message: `Metro waiting on exp://...`

**Driver App URLs:**
- **Tunnel**: `exp://pxoj4gg-anonymous-8082.exp.direct`
- **Local Network**: `exp://192.168.31.97:8082`

---

## 📱 Step 3: Setup Passenger App

### 3.1 Install Dependencies

```bash
cd "d:\public transport\passenger-app"
npm install
```

### 3.2 Start Passenger App

```bash
cd "d:\public transport\passenger-app"
npx expo start --tunnel --port 8083
```

**Expected Output:**
- QR Code displayed
- Message: `Metro waiting on exp://...`

**Passenger App URLs:**
- **Tunnel**: `exp://as6tmc4-anonymous-8083.exp.direct`
- **Local Network**: `exp://192.168.31.97:8083`

---

## 📲 Step 4: Access Apps on Mobile Device

### Option A: Same WiFi Network (Recommended)

1. **Install Expo Go** on Android phone from Google Play Store
2. **Connect to same WiFi** as your computer (192.168.31.x network)
3. **For Driver App**: 
   - Open Expo Go → Enter: `exp://192.168.31.97:8082`
4. **For Passenger App**: 
   - Open Expo Go → Enter: `exp://192.168.31.97:8083`

### Option B: Anywhere Using Tunnel

1. **Install Expo Go** on Android phone
2. **For Driver App**: 
   - Open Expo Go → Scan QR code from terminal OR
   - Enter: `exp://pxoj4gg-anonymous-8082.exp.direct`
3. **For Passenger App**: 
   - Open Expo Go → Scan QR code from terminal OR
   - Enter: `exp://as6tmc4-anonymous-8083.exp.direct`

---

## 👥 Test Accounts

### Driver Accounts

| Email | Password | Bus | Notes |
|-------|----------|-----|-------|
| driver@test.com | password123 | BUS-101 | Primary driver |
| driver2@test.com | password123 | BUS-102 | Second driver |
| driver3@test.com | password123 | BUS-103 | Third driver |

### Passenger Accounts

Use any email/password to register as a new passenger in the app.

---

## ✨ Features

### Driver App Features
- ✅ Login/Register
- ✅ Start and end trips
- ✅ Real-time location tracking (GPS)
- ✅ Trip status management
- ✅ View active trips

### Passenger App Features
- ✅ Login/Register
- ✅ View all active buses on OpenStreetMap
- ✅ Real-time bus location updates
- ✅ Select routes to filter buses
- ✅ Running buses list below route selector
- ✅ Click bus markers to zoom to location
- ✅ Full circle bus markers with emoji

### Real-Time Communication
- ✅ WebSocket (Socket.IO) for instant updates
- ✅ Trip start notifications
- ✅ Trip end notifications
- ✅ Live location updates every 5 seconds

---

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/profile` - Get user profile

### Routes
- `GET /api/routes` - Get all routes

### Trips
- `GET /api/trips/active` - Get active trips
- `POST /api/trips/start` - Start new trip
- `POST /api/trips/end` - End trip

---

## 🛠️ Quick Command Reference

### Start All Servers (From Root Directory)

```bash
# Terminal 1: Start Backend
cd "d:\public transport\backend" && npm start

# Terminal 2: Start Driver App
cd "d:\public transport\driver-app" && npx expo start --tunnel --port 8082

# Terminal 3: Start Passenger App
cd "d:\public transport\passenger-app" && npx expo start --tunnel --port 8083
```

### Kill Specific Ports (If Needed)

```bash
npx kill-port 3000 8082 8083
```

### Clear Expo Cache

```bash
cd "d:\public transport\driver-app" && npx expo start --tunnel --port 8082 --clear
cd "d:\public transport\passenger-app" && npx expo start --tunnel --port 8083 --clear
```

---

## 🧪 Testing with Multiple Drivers

1. **Have multiple devices/emulators** with Expo Go installed
2. **Each device scans** driver app QR code
3. **Login as different drivers**: driver@test.com, driver2@test.com, driver3@test.com
4. **On passenger device**, login once and see all buses from all drivers in real-time
5. **Start trips** from different driver devices
6. **See real-time updates** on passenger app

---

## 🌍 Architecture

### Technology Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| **Runtime** | Node.js | v16+ |
| **Backend Framework** | Express.js | 4.18+ |
| **Database** | MongoDB | Atlas Cloud |
| **WebSocket** | Socket.IO | 4.6+ |
| **Mobile Framework** | React Native | 0.81+ |
| **Expo** | SDK 54 | Latest |
| **Maps** | OpenStreetMap | react-native-maps 1.20+ |
| **Authentication** | JWT | HS256 |

### Project Structure

```
public-transport/
├── backend/                    # Node.js server
│   ├── src/
│   │   ├── server.js          # Main server file
│   │   ├── routes/            # API endpoints
│   │   ├── models/            # MongoDB schemas
│   │   ├── socket/            # WebSocket handlers
│   │   └── middleware/        # Auth & validation
│   ├── .env                   # Environment variables
│   └── package.json
├── driver-app/                # Driver React Native app
│   ├── src/
│   │   ├── screens/           # Driver screens
│   │   ├── services/          # API & Socket services
│   │   └── navigation/        # Navigation stack
│   ├── App.js                 # Entry point
│   └── package.json
├── passenger-app/             # Passenger React Native app
│   ├── src/
│   │   ├── screens/           # Passenger screens
│   │   ├── services/          # API & Socket services
│   │   └── navigation/        # Navigation stack
│   ├── App.js                 # Entry point
│   └── package.json
└── SETUP_AND_RUN.md           # This file
```

---

## 🐛 Troubleshooting

### Issue: "Port already in use"
```bash
npx kill-port 3000 8082 8083
```

### Issue: "Metro Bundler error"
```bash
cd app-directory
npx expo start --tunnel --port XXXX --clear
```

### Issue: "Cannot connect to backend"
- Check backend is running on port 3000
- Verify MongoDB connection string in .env
- Ensure both apps and backend are on same network

### Issue: "Expo Go won't scan QR code"
- Make sure WiFi is on both devices
- Use tunnel URLs instead: `exp://...exp.direct`
- Check internet connection

### Issue: "Real-time updates not working"
- Verify WebSocket connections in console logs
- Check if trip:started/trip:ended events are emitted
- Restart all three servers

---

## 📞 Support

For issues or questions:
1. Check backend logs for errors
2. Check console logs in Expo Go (shake device)
3. Verify all three servers are running
4. Ensure MongoDB connection is active

---

## 📝 Notes

- **Server IP**: 192.168.31.97 (adjust for your network)
- **Backend Port**: 3000
- **Driver App Port**: 8082
- **Passenger App Port**: 8083
- **Database**: MongoDB Atlas (Cloud)
- **Maps**: OpenStreetMap (No API key required)
- **Real-time**: WebSocket via Socket.IO

---

## ✅ Final Checklist

- [ ] Node.js installed
- [ ] MongoDB Atlas account created
- [ ] Backend .env configured
- [ ] Backend running on port 3000
- [ ] Driver app running on port 8082
- [ ] Passenger app running on port 8083
- [ ] Expo Go installed on phone
- [ ] Connected to same WiFi or using tunnel
- [ ] Can access apps via URLs
- [ ] Test accounts created
- [ ] Real-time updates working

---

**Last Updated**: November 10, 2025  
**Version**: 1.0
