# Flexibook - Language Teacher Booking Platform

A full-stack web application for booking language lessons with maximum flexibility. Students can find teachers, choose custom session durations, and manage bookings with integrated payments.

## Features

### For Students
- Browse and search language teachers
- View teacher profiles with ratings and experience
- Book lessons with flexible duration (20/45/60/90 minutes)
- Real-time availability checking
- Secure payment via Stripe
- Automatic calendar integration
- Email notifications and reminders
- Booking history and management

### For Teachers
- Create and manage teaching profile
- Set hourly rates and available languages
- Configure recurring availability schedule
- Buffer time between sessions
- View upcoming lessons and earnings
- Student management dashboard

### Technical Features
- Time zone conversion for global users
- Real-time conflict detection
- Automatic meeting link generation
- Responsive design for mobile and desktop
- Secure authentication with NextAuth.js
- Database transactions for booking integrity

## Tech Stack

- **Frontend**: Next.js 14, React, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: NextAuth.js (email/password + Google OAuth)
- **Payments**: Stripe
- **Email**: Resend
- **Deployment**: Vercel + Neon (PostgreSQL)

## Getting Started

### Prerequisites

- Node.js 18+ 
- PostgreSQL database (or Neon account)
- Stripe account
- Resend account (for emails)
- Google OAuth credentials (optional)

### Installation

1. Clone the repository
\`\`\`bash
git clone <repository-url>
cd CSE499
\`\`\`

2. Install dependencies
\`\`\`bash
npm install
\`\`\`

3. Set up environment variables
\`\`\`bash
cp .env.example .env
\`\`\`

Edit `.env` and fill in your credentials:
- `DATABASE_URL`: Your PostgreSQL connection string
- `NEXTAUTH_SECRET`: Generate with `openssl rand -base64 32`
- `NEXTAUTH_URL`: Your app URL (http://localhost:3000 for local)
- `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`: From Google Cloud Console
- `STRIPE_SECRET_KEY` and `STRIPE_PUBLISHABLE_KEY`: From Stripe Dashboard
- `STRIPE_WEBHOOK_SECRET`: From Stripe CLI or Dashboard
- `RESEND_API_KEY`: From Resend Dashboard
- `FROM_EMAIL`: Verified sender email in Resend

4. Set up the database
\`\`\`bash
npx prisma generate
npx prisma db push
\`\`\`

5. Run the development server
\`\`\`bash
npm run dev
\`\`\`

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

\`\`\`
CSE499/
├── app/                      # Next.js 14 app directory
│   ├── api/                  # API routes
│   │   ├── auth/            # Authentication endpoints
│   │   ├── teachers/        # Teacher management
│   │   ├── availability/    # Availability management
│   │   ├── bookings/        # Booking operations
│   │   ├── payments/        # Payment processing
│   │   └── webhooks/        # Stripe webhooks
│   ├── auth/                # Auth pages (signin/signup)
│   ├── dashboard/           # User dashboard
│   ├── teachers/            # Teacher pages
│   ├── bookings/            # Booking pages
│   └── page.tsx             # Landing page
├── components/              # React components
│   ├── ui/                  # Reusable UI components
│   ├── dashboard/           # Dashboard components
│   ├── navbar.tsx           # Navigation bar
│   └── providers/           # Context providers
├── lib/                     # Utility libraries
│   ├── prisma.ts            # Prisma client
│   ├── auth.ts              # Auth configuration
│   ├── stripe.ts            # Stripe client
│   ├── email.ts             # Email utilities
│   ├── time-utils.ts        # Time handling
│   ├── validations.ts       # Zod schemas
│   └── utils.ts             # General utilities
├── prisma/
│   └── schema.prisma        # Database schema
└── types/                   # TypeScript types
\`\`\`

## Database Schema

### Key Models
- **User**: Base user information with role (STUDENT/TEACHER/ADMIN)
- **Teacher**: Extended profile for teachers
- **Availability**: Recurring or one-time availability slots
- **Booking**: Session bookings with time and status
- **Payment**: Payment records linked to Stripe
- **Notification**: Email notification queue

## API Endpoints

### Authentication
- `POST /api/auth/signup` - Register new user
- `POST /api/auth/signin` - Sign in (NextAuth)
- `POST /api/auth/signout` - Sign out

### Teachers
- `GET /api/teachers` - List all active teachers
- `GET /api/teachers/[id]` - Get teacher details
- `GET /api/teachers/profile` - Get current teacher profile
- `PATCH /api/teachers/profile` - Update teacher profile

### Availability
- `GET /api/availability` - Get teacher availability
- `POST /api/availability` - Add availability slot
- `DELETE /api/availability` - Remove availability slot

### Bookings
- `GET /api/bookings` - List user's bookings
- `POST /api/bookings` - Create new booking
- `GET /api/bookings/[id]` - Get booking details
- `PATCH /api/bookings/[id]` - Update booking (cancel, etc.)

### Payments
- `POST /api/payments/create-checkout` - Create Stripe checkout session
- `POST /api/webhooks/stripe` - Handle Stripe webhooks

## Development

### Running Tests
\`\`\`bash
npm test
\`\`\`

### Database Management
\`\`\`bash
# Open Prisma Studio
npm run db:studio

# Reset database
npx prisma db push --force-reset

# Generate Prisma Client
npm run db:generate
\`\`\`

### Stripe Webhook Testing
\`\`\`bash
# Install Stripe CLI
stripe listen --forward-to localhost:3000/api/webhooks/stripe

# Use the webhook secret from the output in your .env
\`\`\`

## Deployment

### Vercel Deployment

1. Push code to GitHub
2. Import project in Vercel
3. Add environment variables
4. Deploy

### Database Setup (Neon)

1. Create project on Neon
2. Copy connection string to `DATABASE_URL`
3. Run migrations: `npx prisma db push`

### Stripe Webhook

1. Add webhook endpoint in Stripe Dashboard:
   - URL: `https://your-domain.com/api/webhooks/stripe`
   - Events: `checkout.session.completed`, `checkout.session.expired`
2. Copy webhook secret to environment variables

## Security Considerations

- All API routes are protected with session checks
- Payment amounts calculated server-side only
- Time slots locked with database transactions
- Webhook signatures verified
- Passwords hashed with bcrypt (cost factor 12)
- CSRF protection via NextAuth

## Future Enhancements

- [ ] Google Calendar integration
- [ ] Zoom API for meeting links
- [ ] SMS notifications via Twilio
- [ ] Review and rating system
- [ ] Package deals (buy 10 lessons, get discount)
- [ ] Teacher search filters (price, language, rating)
- [ ] Admin dashboard for user management
- [ ] Analytics for teachers (earnings, student retention)
- [ ] Multi-language support (i18n)
- [ ] Mobile app (React Native)

## Contributing

This is a class project for CSE 499. Please follow the coding standards and submit pull requests for review.

## License

MIT License - See LICENSE file for details

## Contact

For questions or support, contact the development team.

