# FUDMA VerifiedRide — Build Prompt

Use this as the project spec/prompt when starting the repo on GitHub or handing it to an AI coding tool (Claude Code, etc).

## Project summary

Build a web app called **FUDMA VerifiedRide** that lets students book guaranteed transport seats through partner motor parks — mainly for end-of-semester travel home, and for part-time working students who need reliable recurring trips. The platform partners with motor park operators so seat capacity is always backed by real vehicles, not just promises.

The app is a **closed portal, not a public site**: no ride listings, pricing, routes, or any other information are visible until the user signs in with a verified account. This is core to the safety promise — every person who can see or book a ride has already been identity-verified.

## Core user roles

1. **Rider** — student booking a seat
2. **Driver** — affiliated with a partner motor park, has a private profile
3. (Future) **Admin** — confirms deposit payments, manages parks/drivers

## Access control

- The only page visible to a signed-out visitor is the **sign-in / sign-up screen**. No routes, prices, ride listings, or driver information are reachable without an authenticated session.
- Sign-up requires personal email verification and a school ID upload; the account is marked **unverified** until the ID is reviewed/approved.
- **Unverified accounts** can log in but should see a "verification pending" state instead of the full portal — not full access, since the whole point of verification is to keep unverified people out of the rider/driver pool.
- Once **verified**, the rider is dropped into the main portal: search, route pricing, recent routes, bookings, etc.
- The **driver portal** is a separate authenticated area — a driver login must not expose rider data, and a rider login must not expose the driver-only profile fields (ID, until a booking is paid).
- Session handling: standard login session/token; consider a "remember me" option so returning students aren't forced to re-verify every visit — only re-authenticate, not re-verify identity each time.

## Rider-facing features

- **Profile & verification**: sign up with personal email, upload school ID card for verification, phone number optional. Profile must be created and verified before a booking can be made.
- **Search & booking form**: From and To fields are dropdown selects of available locations (all Nigerian states), plus a live map picker for choosing the destination visually. Day and time are entered manually.
- **Route pricing**: a section/list showing the price for each available destination.
- **Recent routes**: after login, show the rider's previously searched/booked routes as quick-select chips, with a live availability check.
- **Ride listing**: each ride shows park, vehicle, departure time, fare, and seats left. Each vehicle has a fixed seat capacity; once fully booked, the ride is automatically marked "Full" and can no longer be booked.
- **Booking & deposit**: booking a seat shows a bank information panel (account name, number, amount) for the rider to pay a **15% deposit** of the fare. Balance is paid to the driver on the day of travel.
- **Payment status**: each booking shows a status of **Paid** or **Not paid / Pending** until the deposit is confirmed.
- **Driver reveal**: once a booking's payment status is "Paid," the assigned driver's name, phone number, and photo become visible to that rider. Before payment is confirmed, this info stays hidden.

## Driver-facing features

- **Driver-only portal**, separate from the rider interface.
- Driver profile fields: phone number, car plate number, face photo, and park affiliation.
- This information is private by default — only exposed to a rider once that rider's specific booking is marked "Paid."

## Payment handling

- Riders pay a 15% deposit via bank transfer to details shown at booking time.
- Each booking needs a payment status field (`pending` / `paid`) that can be updated once a transfer is confirmed.
- Note: confirming payment automatically requires integrating a payment gateway with webhook support (e.g. Paystack or Flutterwave) instead of manual bank transfer, if you want this to update in real time without an admin checking manually. Decide early whether v1 uses manual admin confirmation or a payment gateway.

## Map / location picker

- Riders should be able to pick a destination visually on a map, not just by typing an address.
- Requires a mapping service API (Google Maps Platform, Mapbox, or OpenStreetMap/Leaflet as a free alternative) with an API key. Budget for this before building, since most mapping APIs are metered past a free tier.

## Nigerian Locations

The platform supports all 36 states of Nigeria plus the Federal Capital Territory (FCT). See `data/nigerian-locations.json` for the complete list with coordinates.

## Suggested data models

- `User` (id, name, email, phone, role: rider/driver, school_id_url, verified: bool, session_token)
- `Driver` (user_id, plate_number, photo_url, park_id)
- `Park` (id, name, state)
- `Vehicle` (id, park_id, driver_id, type, seat_capacity)
- `Route` (id, from_location, to_location, base_fare)
- `Ride` (id, vehicle_id, route_id, departure_date, departure_time, seats_booked)
- `Booking` (id, ride_id, rider_id, fare, deposit_amount, payment_status, created_at)
- `Location` (id, name, lat, lng, state) — powers the dropdowns and map pins

## Suggested stack (adjust as needed)

- Frontend: React (or plain HTML/CSS/JS for a lean v1) — a working frontend-only prototype already exists and can be used as the visual reference
- Backend: Node.js/Express or similar, with a database (Postgres recommended for relational data like bookings/seats)
- Auth: full login system (not just email link) — email/password or OTP session login, route/page guards so no portal content renders without an authenticated + verified session, plus file upload for school ID
- Map: Leaflet + OpenStreetMap (free) or Google Maps Platform (paid, better UX/search)
- Payments: manual admin confirmation for v1, or Paystack/Flutterwave integration for automatic confirmation later

## Open decisions to make before/while building

- Manual payment confirmation vs. payment gateway integration
- Which mapping service to use, and budget for API usage
- Whether "recent routes" needs to be stored server-side per user, or can be local/session-based for v1
- Refund/cancellation policy for the 15% deposit (not yet defined)
- How long a "verification pending" account can browse a limited/blank state before being approved — and who reviews the uploaded school ID
