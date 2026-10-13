# MediSlot — Doctor Appointment Web App

**MediSlot** is a full-stack healthcare appointment platform. It makes it simple for patients to discover doctors, book appointments, and pay online, while giving doctors and administrators dedicated dashboards to manage their practice and the platform.

The project is split into three independently deployable apps that share one REST API:

| App | Description | Live |
| --- | --- | --- |
| `frontend` | Patient-facing app (browse, book, pay) | https://medi-slot-murex.vercel.app |
| `admin` | Admin + Doctor dashboard | https://medi-slot-2wr6.vercel.app |
| `backend` | Express REST API + MongoDB | https://medislot-lyd4.onrender.com |

## 🛠️ Tech Stack

- **Frontend / Admin**: React 19, Vite, React Router, Tailwind CSS, Axios, React Toastify
- **Backend**: Node.js, Express 5
- **Database**: MongoDB (Mongoose)
- **Auth**: JSON Web Tokens (JWT) with `bcrypt` password hashing
- **Media**: Cloudinary (doctor & profile images)
- **Payments**: Razorpay
- **Deployment**: Vercel (frontend + admin), Render (backend), MongoDB Atlas

## 🔑 Key Features

### Patient
- Sign up / log in and manage a personal profile (name, email, address, gender, birthday, profile picture).
- Search doctors by specialty and view detailed doctor profiles.
- Book appointments by date, time, and payment method (Cash or Razorpay).
- Pay securely through Razorpay and view upcoming/past appointments.
- Cancel or reschedule appointments.

### Doctor
- Dedicated dashboard with earnings, patient count, appointment count, and latest bookings.
- View appointment details (patient info, payment mode, status).
- Mark appointments as completed or cancel them.
- Update profile details (description, fees, address, availability).

### Admin
- Dashboard analytics: total doctors, patients, appointments, and recent bookings.
- Create and manage doctor profiles (image, specialty, degree, experience, address, fees, etc.).
- View and manage every appointment (cancel or mark as completed).

## 📦 Project Structure

```plaintext
MediSlot/
├── frontend/                 # Patient app (React + Vite)
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   └── main.jsx
│   ├── vercel.json           # SPA rewrites
│   └── .env.example
│
├── admin/                    # Admin + Doctor dashboard (React + Vite)
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   └── main.jsx
│   ├── vercel.json           # SPA rewrites
│   └── .env.example
│
└── backend/                  # REST API (Node + Express + MongoDB)
    ├── config/               # DB + Cloudinary config
    ├── controllers/
    ├── middleware/           # Auth + error handling
    ├── models/               # Mongoose schemas
    ├── routes/
    ├── server.js
    └── .env.example
```

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- A MongoDB database (local or MongoDB Atlas)
- Cloudinary account (for image uploads)
- Razorpay account (for payments)

### 1. Clone the repository

```bash
git clone https://github.com/anumukul/MediSlot.git
cd MediSlot
```

### 2. Configure environment variables

Each app has its own env file. Copy the templates and fill in your values:

```bash
cp backend/.env.example  backend/.env
cp frontend/.env.example frontend/.env
cp admin/.env.example    admin/.env
```

**`backend/.env`**

| Variable | Description |
| --- | --- |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret used to sign JWTs |
| `CLOUDINARY_NAME` | Cloudinary cloud name |
| `CLOUDINARY_API_KEY` | Cloudinary API key |
| `CLOUDINARY_SECRET_KEY` | Cloudinary API secret |
| `ADMIN_EMAIL` | Admin login email |
| `ADMIN_PASSWORD` | Admin login password |
| `RAZORPAY_KEY_ID` | Razorpay key id |
| `RAZORPAY_KEY_SECRET` | Razorpay key secret |
| `CURRENCY` | Display currency (e.g. `INR`) |
| `PORT` | Local port (default `4000`) |

**`frontend/.env`**

| Variable | Description |
| --- | --- |
| `VITE_BACKEND_URL` | Backend API base URL (no trailing slash) |
| `VITE_RAZORPAY_KEY_ID` | Razorpay public key id |
| `VITE_ADMIN_URL` | URL of the admin panel |

**`admin/.env`**

| Variable | Description |
| --- | --- |
| `VITE_BACKEND_URL` | Backend API base URL (no trailing slash) |
| `VITE_CURRENCY` | Display currency |
| `VITE_USER_URL` | URL of the patient frontend |

> All `VITE_*` values are baked in at build time, so update and redeploy after changing them.

### 3. Install dependencies & run

Run each app in its own terminal:

```bash
# Backend  -> http://localhost:4000
cd backend
npm install
npm run server      # nodemon (dev) — use `npm start` for production

# Frontend -> http://localhost:5173
cd frontend
npm install
npm run dev

# Admin    -> http://localhost:5174
cd admin
npm install
npm run dev
```

## ☁️ Deployment

The apps are deployed independently and wired together through environment variables.

### Backend → Render (Web Service)
- **Root Directory**: `backend`
- **Build Command**: `npm install`
- **Start Command**: `npm start`
- **Environment**: add every variable from `backend/.env` (do **not** set `PORT` — Render provides it).
- Allow Render to reach MongoDB Atlas via **Network Access → `0.0.0.0/0`**.

### Frontend & Admin → Vercel
Create two Vercel projects from this repo:

| Project | Root Directory | Environment Variables |
| --- | --- | --- |
| Frontend | `frontend` | `VITE_BACKEND_URL`, `VITE_RAZORPAY_KEY_ID`, `VITE_ADMIN_URL` |
| Admin | `admin` | `VITE_BACKEND_URL`, `VITE_CURRENCY`, `VITE_USER_URL` |

- Framework preset: **Vite** (build `npm run build`, output `dist`).
- `vercel.json` already handles SPA routing.
- After the first deploy, set `VITE_ADMIN_URL` (frontend) and `VITE_USER_URL` (admin) to the deployed panel URLs and **redeploy**, since Vite inlines env vars at build time.

## 📝 Notes

- Payments use **Razorpay**. Test keys (`rzp_test_*`) work end-to-end; live payments require a verified Razorpay account.
- Backend CORS is currently open so any Vercel origin can call the API.
- Render's free tier sleeps after inactivity, so the first request after idle may be slow.

## 🤝 Contributing

Contributions are welcome! Feel free to open issues, fork the repo, and submit pull requests.

## 🌟 Acknowledgements

Thanks to the teams behind MongoDB, Express, React, Node.js, Vite, Tailwind CSS, Cloudinary, and Razorpay for their excellent tools and libraries.
