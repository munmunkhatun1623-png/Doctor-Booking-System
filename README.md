# MediBook — Doctor Booking System (React)

A full React application for browsing doctors, checking live availability, and
booking appointments. Built with Vite + React Router, and designed as a
teaching example of core React concepts.

## Concepts demonstrated

| Concept | Where |
|---|---|
| **Component-based architecture** | `src/components/*` (Navbar, DoctorCard, Button, Rating, Loader, SearchBar, SpecialtyFilter, EmptyState) and `src/pages/*` |
| **JSX** | Every `.jsx` file |
| **Props** | `DoctorCard`, `Button`, `Rating`, `SearchBar`, `SpecialtyFilter`, `EmptyState` all render purely from props |
| **State management** | `useState` (search/filters/forms), `useReducer` + Context (`AppointmentsContext.jsx`) for global appointment state |
| **Routing** | `react-router-dom` — `App.jsx` defines `/`, `/doctors/:id`, `/book/:id`, `/appointments`, and a `*` 404 route |
| **Reusable components** | `Button` and `Rating` accept variants/props and are reused across pages |
| **Multiple styling approaches** | Global CSS (`index.css`), **CSS Modules** (`*.module.css`), **styled-components** (`Button.jsx`), inline styles (`Rating.jsx`, `EmptyState.jsx`) |
| **Real-time / API data handling** | `src/api/doctorsApi.js` (simulated network latency, slot-taken race condition), `src/hooks/useFetch.js` (loading/error/data pattern) |

## Project structure

```
src/
  api/doctorsApi.js          # mock REST-like API (Promise-based, artificial delay)
  data/doctors.js            # seed dataset (10 doctors, generated availability)
  context/AppointmentsContext.jsx  # global state via Context + useReducer
  hooks/useFetch.js          # reusable async data-fetching hook
  components/                # small reusable UI pieces
  pages/
    Home/                    # search + filter + doctor grid
    DoctorDetails/           # doctor profile + availability preview
    BookAppointment/         # date -> live slot check -> patient form -> confirm
    MyAppointments/          # list + cancel appointments
    NotFound/                # 404 route
```

## How the "API integration" works

There's no real backend here — `src/api/doctorsApi.js` simulates one:

- Every function is `async` and awaits an artificial delay (`setTimeout`),
  so the UI shows genuine loading states, not instant fake data.
- Booked slots are persisted to `localStorage`, so availability updates
  immediately (in the same session) after a booking — reload the page and
  your appointments are still there.
- `bookAppointment` can throw a `SLOT_TAKEN` error if a slot was booked in
  the meantime, and the UI handles that gracefully by refreshing availability.

Swapping this for a real backend later just means replacing the function
bodies in `doctorsApi.js` with real `fetch('/api/...')` calls — every
component already consumes it through the same async contract.

## Getting started

```bash
npm install
npm run dev
```

Then open the URL Vite prints (typically http://localhost:5173).

To build for production:

```bash
npm run build
npm run preview
```

## Try it out

1. On the **Home** page, search or filter by specialty.
2. Click **View Profile** to see a doctor's bio and upcoming availability.
3. Click **Book Now**, pick a date (slots load live from the mock API),
   pick an open time slot, fill in patient details, and confirm.
4. Go to **My Appointments** to see your booking (persists via
   localStorage) and cancel it if needed.
