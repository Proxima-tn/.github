<div align="center">

# Proxima

### La proximité réinventée.

Proxima is a three-application platform that connects local businesses with nearby customers.
It includes dedicated mobile experiences for businesses and customers, backed by a Django API.

</div>

## Project Overview

| Project | Purpose | Technology |
| --- | --- | --- |
| `business` | Merchant workspace for managing offers, scanning customer QR codes, viewing activity, and accessing business settings | Expo SDK 57, React Native, Expo Router, TypeScript, Gluestack UI |
| `customer` | Customer experience for discovering nearby offers, scanning merchant QR codes, saving favorites, and managing an account | Expo SDK 57, React Native, Expo Router, TypeScript, Gluestack UI |
| `server` | Backend API and health endpoint shared by both mobile applications | Django 5.2, Python 3.12+, PostgreSQL |

## Architecture

```text
business/   ─┐
			 ├── HTTP API ── server/ (Django)
customer/  ──┘
```

The mobile applications are separate Expo projects so each audience can have its own navigation, content, branding, and future permissions while sharing the same API. The current server root endpoint returns a health response:

```json
{"status": "ok"}
```

## Business App

The Business app is designed for local merchants. Its navigation currently includes:

- **Accueil**: merchant greeting, activity statistics, and recent scans.
- **Offres**: active promotions and offer management entry points.
- **Scanner**: QR code scanning workspace and recent validations.
- **Favoris**: frequently returning customers.
- **Plus**: business settings, statistics, help, about, and sign out.
- **Profil**: business profile and billing menu opened from the header.

The Business app uses a purple brand color and a dark-only interface.

## Customer App

The Customer app is designed for people discovering and using local offers. Its navigation currently includes:

- **Accueil**: nearby businesses, available offers, loyalty points, and favorites.
- **Offres**: redeemable offers from nearby businesses.
- **Scanner**: QR code scanning workspace for claiming an offer.
- **Favoris**: saved local businesses.
- **Plus**: account, notifications, help, about, and sign out.
- **Profil**: customer profile and payment menu opened from the header.

The Customer app uses a light blue brand color and a dark-only interface.

## Shared Mobile Experience

Both mobile apps use:

- Expo Router file-based navigation.
- A shared five-item bottom navigation pattern with an elevated scanner action.
- A Proxima header with the app logo, slogan, audience label, and profile action.
- Dark mode enforced at the native and JavaScript levels.
- Gluestack UI for the component theme and layout primitives.
- `expo-symbols` for platform-aware icons.
- Animated page transitions when switching tabs.
- An animated splash screen with the Proxima logo, rotating loader, and changing quotes.
- An `EXPO_PUBLIC_SERVER_API` environment variable for API access.

## Server

The server is a minimal Django API foundation. The `api` application currently provides the root health endpoint at `/`; models and migrations have not been added yet.

Configuration is read from environment variables:

- `DJANGO_SECRET_KEY`
- `DJANGO_DEBUG`
- `DJANGO_ALLOWED_HOSTS`
- `POSTGRES_DB`
- `POSTGRES_USER`
- `POSTGRES_PASSWORD`
- `POSTGRES_HOST`
- `POSTGRES_PORT`

See [server/README.md](../server/README.md) for the backend-specific setup notes.

## Requirements

- Node.js and npm
- Python 3.12 or newer
- PostgreSQL 16 or newer for the server
- Android Studio/emulator, an iOS simulator, or Expo Go for mobile development

## Getting Started

### Business

```powershell
cd business
npm install
npx expo start
```

### Customer

```powershell
cd customer
npm install
npx expo start
```

Both mobile apps use the following local API configuration in `.env`:

```env
EXPO_PUBLIC_SERVER_API=http://192.168.100.153:8000
```

For a physical device, replace the address with the server's reachable LAN address. Do not use `localhost` from a physical phone unless the API is running on the phone itself.

### Server

```powershell
cd server
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python manage.py runserver
```

The local API is available at `http://127.0.0.1:8000/` by default. Run `python manage.py check` to validate the Django configuration.

## Repository Layout

```text
Proxima/
├── business/       Merchant Expo application
├── customer/       Customer Expo application
├── server/         Django API
└── .github/        Organization documentation
```

Each application has its own dependencies, configuration, assets, source tree, and license. Changes to shared behavior should be applied to both mobile applications unless the behavior is intentionally audience-specific.

## Current Scope

The mobile screens currently contain representative placeholder content to establish the product structure and visual direction. The backend is currently a health-check foundation. Authentication, persistent business/customer data, QR validation logic, offer persistence, and production deployment configuration are planned application work rather than implemented functionality at this stage.
