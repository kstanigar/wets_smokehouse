# WET SMOKEHOUSE BBQ

A weekend-only BBQ and catering business website with VIP email blasts, menu management, and future payment integration.

## 🔥 About

WET SMOKEHOUSE is a weekend BBQ operation serving authentic smoked meats and sides. This website enables:
- Menu management from mobile devices
- VIP customer email blasts with photos
- Customer email capture for VIP list
- (Future) Online ordering via Square
- (Future) SMS notifications via Twilio

## 🛠️ Tech Stack

- **Frontend:** Next.js 14, TypeScript, Tailwind CSS
- **Database:** Supabase (PostgreSQL)
- **Hosting:** Vercel
- **Email:** SendGrid (free tier)
- **Future:** Twilio (SMS), Square (payments)

## 📁 Project Structure

```
wet_smokehouse/
├── PROJECT-DOCS/           # Complete project documentation
│   ├── RULES.md           # Development rules and governance
│   ├── MASTER-PLAN.md     # Project overview and status
│   ├── QUICKSTART.md      # Quick start for new AI agents
│   ├── phases/            # Phase-specific plans
│   ├── sessions/          # Session logs and approvals
│   └── decisions/         # Architecture and tech decisions
├── src/                   # Source code (Phase 1+)
├── public/                # Static assets
└── README.md             # This file
```

## 📚 Documentation

**New to this project?** Start here:
1. Read [PROJECT-DOCS/QUICKSTART.md](./PROJECT-DOCS/QUICKSTART.md) (10 min)
2. Read [PROJECT-DOCS/RULES.md](./PROJECT-DOCS/RULES.md) (5 min)
3. Check [PROJECT-DOCS/MASTER-PLAN.md](./PROJECT-DOCS/MASTER-PLAN.md) for current status

**All documentation follows reverse chronological format** (newest information at top).

## 🚀 Development

### Prerequisites
- Node.js 18+
- npm or yarn
- Supabase account (free tier)
- Vercel account (free tier)
- SendGrid account (free tier)

### Setup (Phase 1+)
```bash
# Clone repo
git clone <repo-url>
cd wet_smokehouse

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Add your Supabase and SendGrid credentials

# Run development server
npm run dev
```

Visit `http://localhost:3000`

## 📋 Current Status

**Phase:** Planning Complete → Phase 1 Starting
**Launch Timeline:** 4 weeks
**Budget:** $0/month (all free tiers)

See [PROJECT-DOCS/MASTER-PLAN.md](./PROJECT-DOCS/MASTER-PLAN.md) for detailed status.

## 🔐 Security

- Never commit `.env` files (use `.env.example` template)
- All secrets stored in environment variables
- Admin routes protected with Supabase Auth
- Row-Level Security (RLS) enabled on all tables

## 📱 Owner Interface

The admin interface is designed for mobile-first management:
- Update menu availability (checkboxes)
- Send VIP email blasts (message + photo)
- View orders in Google Sheets (when payments added)

## 🤝 Contributing

This is a private project. All development follows the approval workflow defined in [PROJECT-DOCS/RULES.md](./PROJECT-DOCS/RULES.md).

## 📄 License

Private - All Rights Reserved

---

**Built with ❤️ and 🔥 by Claude Code**