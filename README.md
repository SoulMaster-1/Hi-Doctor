# Doctors Appointment Platform

A full-stack healthcare appointment platform that connects patients with verified doctors for online consultations. Patients can discover doctors by specialty, book available time slots using consultation credits, and join secure video calls. Doctors can manage availability, track appointments, add consultation notes, and request payouts, while admins can verify doctors and process payout requests.

## Highlights

- Role-based platform for patients, doctors, and admins.
- Doctor discovery by specialty with verified provider profiles.
- Appointment booking with 30-minute availability slots and overlap prevention.
- Credit-based consultation system with monthly plan allocation through Clerk Billing.
- Secure video consultation flow using Vonage Video API.
- Doctor earnings dashboard with payout requests and admin payout approval.
- Admin dashboard for doctor verification, suspension, and payout management.

## Tech Stack

- **Frontend:** Next.js 15, React 19, Tailwind CSS, Radix UI, Lucide React
- **Backend:** Next.js Server Actions, Prisma ORM
- **Database:** PostgreSQL
- **Authentication & Billing:** Clerk
- **Video Calls:** Vonage Video API / OpenTok
- **Validation & Forms:** Zod, React Hook Form
- **UI Feedback:** Sonner toast notifications

## Core Features

### Patient Experience

- Sign up and select the patient role during onboarding.
- Browse verified doctors by specialty.
- View doctor profile, experience, description, and available slots.
- Book appointments using credits.
- View upcoming and past appointments.
- Join video consultations from the appointments page.
- Cancel appointments and receive credit refunds.

### Doctor Experience

- Apply as a doctor with specialty, experience, credentials, and profile description.
- Wait for admin verification before appearing in patient search.
- Set consultation availability.
- View scheduled appointments with patient details.
- Add notes after consultations.
- Mark appointments as completed after the scheduled end time.
- Track total earnings, monthly earnings, available credits, and payout value.
- Request PayPal payouts for earned credits.

### Admin Experience

- Review pending doctor applications.
- Approve or reject doctor verification requests.
- Suspend or reinstate doctors.
- Review pending payout requests.
- Approve payouts and deduct processed credits from doctor balances.

## How The Credit System Works

- Each appointment costs **2 credits**.
- Patients receive credits based on their Clerk subscription plan.
- Booking an appointment deducts credits from the patient and transfers them to the doctor.
- Cancelled appointments refund the patient and reverse the doctor credit transfer.
- Doctors can request payouts for earned credits.
- The platform uses a payout model of **$10 per credit**, with **$8 paid to the doctor** and **$2 retained as platform fee**.

## Project Structure

```text
.
├── actions/                  # Server actions for bookings, credits, payouts, admins, doctors
├── app/                      # Next.js App Router pages and layouts
│   ├── (auth)/               # Clerk sign-in and sign-up routes
│   └── (main)/               # Protected app routes
├── components/               # Shared UI and domain components
├── hooks/                    # Custom React hooks
├── lib/                      # Prisma client, user sync, schemas, utilities, static data
├── prisma/                   # Prisma database schema
└── public/                   # Static assets
```

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd doctors-appointment-platform-main
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file in the project root and add the required credentials.

```env
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE"

NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="your_clerk_publishable_key"
CLERK_SECRET_KEY="your_clerk_secret_key"

NEXT_PUBLIC_VONAGE_APPLICATION_ID="your_vonage_application_id"
VONAGE_APPLICATION_ID="your_vonage_application_id"
VONAGE_PRIVATE_KEY="path/to/private.key"
```

`VONAGE_PRIVATE_KEY` can be either the private key content or a path to a key file. If you store the key file locally, keep it out of version control.

### 4. Set up the database

```bash
npx prisma generate
npx prisma db push
```

### 5. Run the development server

```bash
npm run dev
```

Open `http://localhost:3000` in your browser.

## Important Routes

| Route | Purpose |
| --- | --- |
| `/` | Landing page with platform overview and pricing |
| `/onboarding` | Role selection for patient or doctor |
| `/doctors` | Browse doctors and specialties |
| `/doctors/[specialty]/[id]` | Doctor profile and appointment booking |
| `/appointments` | Patient appointment management |
| `/doctor` | Doctor dashboard |
| `/doctor/verification` | Doctor verification status |
| `/admin` | Admin dashboard |
| `/video-call` | Secure video consultation page |

## Database Models

The Prisma schema includes:

- `User` for patient, doctor, and admin profiles
- `Availability` for doctor consultation windows
- `Appointment` for scheduled, completed, and cancelled consultations
- `CreditTransaction` for purchases, deductions, refunds, and admin adjustments
- `Payout` for doctor payout requests and admin processing

## Security And Access Control

- Protected routes are enforced through Clerk middleware.
- Users must be authenticated to access doctors, appointments, doctor dashboard, admin dashboard, and video calls.
- Role checks are handled in server actions before sensitive operations.
- Admin-only actions verify the current user role before reading or mutating doctor and payout data.
- Video tokens are generated only for users who belong to the appointment.

## Resume Points

- Built a full-stack doctor appointment platform using Next.js, React, Prisma, PostgreSQL, and Clerk, enabling patients, doctors, and admins to complete role-specific healthcare workflows from one application.
- Implemented secure authentication, onboarding, and route protection with Clerk, separating patient, doctor, and admin access to reduce unauthorized access risk across sensitive healthcare flows.
- Designed an appointment booking system with doctor availability, 30-minute slot generation, conflict detection, cancellation handling, and credit refunds to improve booking reliability.
- Integrated Vonage Video API to support real-time online consultations, including appointment-based session creation and secure token generation for authorized participants.
- Developed a credit-based consultation and earnings model with transactional Prisma updates, ensuring patient deductions, doctor credit additions, and cancellation reversals remain consistent.
- Created doctor and admin dashboards for verification, appointment management, earnings tracking, payout requests, and payout approvals, improving operational control for marketplace-style healthcare workflows.
- Modeled the database with Prisma relations and indexes for users, appointments, availability, credit transactions, and payouts, making the platform scalable for multi-role scheduling and payment operations.

## Future Enhancements

- Email and SMS reminders for upcoming appointments.
- Prescription upload and secure medical document sharing.
- Patient reviews and doctor ratings.
- Advanced calendar integration for doctors.
- Automated payout processing through a payment provider.

## Author

Made by **Swastik Gupta**, IIIT Kalyani.
