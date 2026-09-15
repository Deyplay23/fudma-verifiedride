# FUDMA VerifiedRide Project Structure

```
fudma-verifiedride/
├── BUILD_PROMPT.md              # Project specification
├── data/
│   └── nigerian-locations.json  # All 36 states + FCT with coordinates
├── models/
│   └── data-models.ts           # TypeScript interfaces for all entities
├── frontend/                     # React or vanilla JS/HTML/CSS
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── auth/
│   │   │   │   ├── SignIn.tsx
│   │   │   │   ├── SignUp.tsx
│   │   │   │   └── VerificationPending.tsx
│   │   │   ├── rider/
│   │   │   │   ├── SearchRides.tsx
│   │   │   │   ├── RideListing.tsx
│   │   │   │   ├── BookingSummary.tsx
│   │   │   │   ├── DepositPayment.tsx
│   │   │   │   └── MyBookings.tsx
│   │   │   ├── driver/
│   │   │   │   ├── DriverProfile.tsx
│   │   │   │   ├── DriverDashboard.tsx
│   │   │   │   └── RidesList.tsx
│   │   │   ├── map/
│   │   │   │   └── LocationPicker.tsx
│   │   │   └── shared/
│   │   │       ├── Header.tsx
│   │   │       ├── Footer.tsx
│   │   │       └── Layout.tsx
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── services/
│   │   │   └── api.ts           # API client
│   │   ├── styles/
│   │   └── App.tsx
│   └── package.json
├── backend/                      # Node.js/Express
│   ├── src/
│   │   ├── routes/
│   │   │   ├── auth.ts
│   │   │   ├── riders.ts
│   │   │   ├── drivers.ts
│   │   │   ├── rides.ts
│   │   │   ├── bookings.ts
│   │   │   └── admin.ts
│   │   ├── controllers/
│   │   ├── middleware/
│   │   │   ├── authMiddleware.ts
│   │   │   └── verificationMiddleware.ts
│   │   ├── services/
│   │   │   ├── userService.ts
│   │   │   ├── bookingService.ts
│   │   │   ├── paymentService.ts
│   │   │   └── emailService.ts
│   │   ├── database/
│   │   │   ├── migrations/
│   │   │   ├── seeds/
│   │   │   └── db.ts
│   │   ├── utils/
│   │   └── server.ts
│   ├── .env.example
│   └── package.json
├── docs/
│   ├── API.md                   # API documentation
│   ├── SETUP.md                 # Setup & installation guide
│   └── ARCHITECTURE.md          # System architecture
└── README.md                     # Project overview
```

## Key Directories

- **data/**: Static data files (Nigerian locations, seed data)
- **models/**: TypeScript interfaces for all data entities
- **frontend/**: React-based UI with components for riders, drivers, and auth
- **backend/**: Express API server with routes, controllers, and services
- **docs/**: Project documentation

## Getting Started

1. Clone the repo
2. Follow setup guide in `docs/SETUP.md`
3. Use `BUILD_PROMPT.md` for context when implementing features
4. Reference `models/data-models.ts` for database schema
5. Use `data/nigerian-locations.json` for location dropdowns and map
