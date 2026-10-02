# 🕰️ Digital Time Capsule & Future Message System

A full-stack web application that lets you create digital time capsules containing messages, photos, and videos — locked until a future date you choose.

![Tech Stack](https://img.shields.io/badge/Stack-React%20%7C%20Node.js%20%7C%20MongoDB-FF2D75?style=for-the-badge)

---

## ✨ Features

- 🔒 **Secure Time-Locked Capsules** — Content is never exposed by the API until unlock time
- 📸 **Image & Video Uploads** — Attach photos and videos to your capsules
- ⏱️ **Live Countdown Timer** — Watch the seconds tick down to your unlock date
- 🎉 **Capsule Reveal Animation** — Beautiful reveal experience when your capsule unlocks
- 🔔 **In-App Notifications** — Get notified the moment your capsule is ready
- 🤖 **AI Message Assistant** — Curated message suggestions to inspire your writing
- 👤 **User Authentication** — JWT-based secure login and registration
- 🛡️ **Admin Dashboard** — Separate admin panel for system management
- 📱 **Fully Responsive** — Works on mobile, tablet, laptop, and desktop
- 🌙 **Pink + Black Theme** — Elegant, premium, modern design

---

## 🗂️ Project Structure

```
Digital time capsule fms/
├── backend/                    # Node.js + Express API
│   ├── config/
│   │   └── db.js               # MongoDB connection
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── capsuleController.js
│   │   ├── notificationController.js
│   │   ├── userController.js
│   │   └── adminController.js
│   ├── middleware/
│   │   ├── auth.js             # JWT protect + adminOnly
│   │   ├── upload.js           # Multer file uploads
│   │   └── validate.js         # express-validator rules
│   ├── models/
│   │   ├── User.js
│   │   ├── TimeCapsule.js
│   │   └── Notification.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── capsuleRoutes.js
│   │   ├── notificationRoutes.js
│   │   ├── userRoutes.js
│   │   └── adminRoutes.js
│   ├── services/
│   │   └── capsuleService.js   # Auto-unlock cron logic
│   ├── utils/
│   │   ├── fileUtils.js
│   │   └── responseUtils.js
│   ├── uploads/                # Uploaded files stored here
│   ├── .env                    # Environment variables
│   ├── .env.example
│   └── server.js               # Entry point
│
└── frontend/                   # React + Vite + Tailwind
    └── src/
        ├── components/
        │   ├── capsule/        # CapsuleCard, CountdownTimer, AIAssistant, Filters
        │   ├── layout/         # Navbar, Sidebar
        │   ├── notifications/  # NotificationBell
        │   └── ui/             # Button, Input, Modal, Badge, EmptyState, Skeletons
        ├── context/
        │   ├── AuthContext.jsx
        │   └── NotificationContext.jsx
        ├── hooks/
        │   └── useCountdown.js
        ├── layouts/
        │   ├── DashboardLayout.jsx
        │   └── MainLayout.jsx
        ├── pages/
        │   ├── LandingPage.jsx
        │   ├── auth/           # Login, Register
        │   ├── capsule/        # Create, Detail, MyCapsules
        │   ├── dashboard/      # Dashboard
        │   ├── notifications/
        │   ├── profile/
        │   ├── settings/
        │   └── admin/          # AdminDashboard
        ├── services/
        │   └── api.js          # Axios + all API services
        └── utils/
            └── helpers.js
```

---

## 🚀 Setup Instructions

### Prerequisites

- **Node.js** v18+ — [Download](https://nodejs.org)
- **MongoDB** — [Download Community](https://www.mongodb.com/try/download/community) or use [MongoDB Atlas](https://cloud.mongodb.com)
- **Git** — [Download](https://git-scm.com)

---

### 1. Clone / Open the Project

```bash
# Navigate to the project folder
cd "Digital time capsule fms"
```

---

### 2. Backend Setup

```bash
cd backend
```

#### Install dependencies
```bash
npm install
```

#### Configure environment variables
The `.env` file is pre-configured for local development:
```env
MONGO_URI=mongodb://localhost:27017/digital-time-capsule
JWT_SECRET=timecapsule_secret_key_2024_change_in_production
JWT_EXPIRE=7d
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

> ⚠️ **Production**: Change `JWT_SECRET` to a strong random string before deploying.

#### Start MongoDB
Make sure MongoDB is running locally:
```bash
# Windows (if installed as service)
net start MongoDB

# Or start manually
mongod
```

#### Start the backend server
```bash
npm run dev
```

The API will be available at `http://localhost:5000`

---

### 3. Frontend Setup

Open a new terminal:

```bash
cd frontend
npm install
npm run dev
```

The app will open at `http://localhost:5173`

---

### 4. Create an Admin Account

After starting both servers, register a normal account, then manually update the role in MongoDB:

**Option A — MongoDB Compass:**
1. Open MongoDB Compass
2. Connect to `mongodb://localhost:27017`
3. Open `digital-time-capsule` → `users`
4. Find your user → Edit → change `role` from `"user"` to `"admin"`

**Option B — MongoDB Shell:**
```bash
mongosh
use digital-time-capsule
db.users.updateOne({ email: "your@email.com" }, { $set: { role: "admin" } })
```

---

## 🔑 API Endpoints

### Authentication
| Method | Route | Description |
|--------|-------|-------------|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login user |
| GET | `/api/auth/me` | Get current user (protected) |

### Time Capsules
| Method | Route | Description |
|--------|-------|-------------|
| POST | `/api/capsules` | Create capsule (multipart) |
| GET | `/api/capsules` | Get all user capsules |
| GET | `/api/capsules/:id` | Get single capsule (**locked content hidden**) |
| GET | `/api/capsules/:id/status` | Check if capsule is unlocked |
| PUT | `/api/capsules/:id` | Update capsule |
| DELETE | `/api/capsules/:id` | Delete capsule |

### Notifications
| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/notifications` | Get all notifications |
| GET | `/api/notifications/unread-count` | Get unread count |
| PUT | `/api/notifications/mark-all-read` | Mark all as read |
| PUT | `/api/notifications/:id/read` | Mark one as read |
| DELETE | `/api/notifications/:id` | Delete notification |

### User Profile
| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/users/profile` | Get profile + stats |
| PUT | `/api/users/profile` | Update profile / avatar |
| PUT | `/api/users/change-password` | Change password |

### Admin (Admin role required)
| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/admin/stats` | System statistics |
| GET | `/api/admin/users` | All users |
| DELETE | `/api/admin/users/:id` | Delete user |
| GET | `/api/admin/capsules` | All capsules |
| DELETE | `/api/admin/capsules/:id` | Delete any capsule |
| GET | `/api/admin/activity` | Recent activity |

---

## 🔒 Security Architecture

The most critical security feature is **backend-enforced content protection**:

```
GET /api/capsules/:id
├── IF current time < unlockAt  →  Returns ONLY: { title, category, createdAt, unlockAt, status: "LOCKED" }
│                                   ❌ NEVER returns: message, imageUrl, videoUrl
└── IF current time >= unlockAt →  Returns full capsule content
```

This is verified **server-side on every request** — not by CSS or JavaScript hiding.

Additional security:
- Passwords hashed with **bcrypt** (12 salt rounds)
- **JWT** authentication on all protected routes
- Rate limiting on auth endpoints (20 req / 15 min)
- Helmet.js security headers
- File type validation (whitelist only)
- 50MB file size limit
- Users can **only access their own capsules**

---

## ⏰ Auto-Unlock Cron Job

The backend runs a cron job **every minute** that:
1. Finds all capsules where `unlockAt <= now` and `status = LOCKED`
2. Updates them to `status = UNLOCKED`, `isLocked = false`
3. Creates in-app notifications for each user

---

## 🎨 Design System

| Token | Value | Usage |
|-------|-------|-------|
| `pink-primary` | `#FF2D75` | Buttons, active states, highlights |
| `pink-bright` | `#FF4F91` | Hover states, gradients |
| `pink-soft` | `#FF8FB3` | Secondary accents |
| `dark-bg` | `#0A0A0A` | Page background |
| `dark-black` | `#111111` | Nested dark sections |
| `dark-card` | `#1A1A1A` | Cards, inputs |
| `dark-border` | `#2A2A2A` | Borders |

---

## 📋 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Tailwind CSS 3 |
| Routing | React Router DOM v6 |
| HTTP | Axios |
| Icons | Lucide React |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Auth | JWT, bcryptjs |
| Uploads | Multer |
| Validation | express-validator |
| Scheduling | node-cron |
| Security | Helmet.js, express-rate-limit |

---

## 🧪 Testing Checklist

- [ ] Register a new account
- [ ] Login with email/password
- [ ] Create a capsule with text message
- [ ] Create a capsule with image upload
- [ ] Create a capsule with video upload
- [ ] Verify locked capsule hides content in UI
- [ ] Verify `/api/capsules/:id` does NOT return message when locked
- [ ] Create a capsule unlocking in ~1 minute to test countdown → reveal
- [ ] Check notification bell shows unread count
- [ ] Mark notifications as read
- [ ] Update profile name and avatar
- [ ] Change password
- [ ] Test admin dashboard (after setting role to admin)
- [ ] Test admin can delete users and capsules
- [ ] Test responsive layout on mobile

---

## 🛠️ Common Issues

**MongoDB connection failed:**
- Make sure MongoDB is running: `mongod`
- Check `MONGO_URI` in `.env`

**CORS errors:**
- Make sure `FRONTEND_URL=http://localhost:5173` in backend `.env`
- Make sure frontend is running on port 5173

**File uploads not showing:**
- The backend serves uploads at `http://localhost:5000/uploads/`
- Make sure the `uploads/` folder exists in the backend

**Capsule not unlocking:**
- The cron job runs every minute
- Check backend console for unlock logs
- Verify `unlockAt` is in the past

---

## 📄 License

MIT — Free for personal and educational use.
