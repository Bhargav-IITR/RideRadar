# 🚕 RideRadar

A React Native mobile app that compares cab fares across **Ola** and **Uber** in real time — so you always book the cheapest ride.

---

## 📱 Features

- 📍 Pickup & drop location search with autocomplete
- 💰 Real-time fare comparison across Ola and Uber
- 🚗 Side-by-side comparison for Mini, Sedan, SUV, and Auto categories
- ✅ "Best Deal" badge highlighting the cheapest option
- ⚡ Surge pricing indicator
- 🔗 One-tap deep link to book directly in the respective app
- 🕘 Search history with saved fare snapshots

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | React Native (Expo) |
| Navigation | React Navigation (Stack + Bottom Tabs) |
| Location | Google Places API |
| Fare Data | Uber Rides API + Ola API |
| Database | Supabase (search history) |
| Language | JavaScript (ES6+) |

---

## 📁 Project Structure

```
rideradar/
├── src/
│   ├── screens/
│   │   ├── HomeScreen.jsx
│   │   ├── ResultsScreen.jsx
│   │   ├── DetailScreen.jsx
│   │   └── HistoryScreen.jsx
│   ├── components/
│   │   ├── PlatformCard.jsx
│   │   ├── LocationSearchBar.jsx
│   │   └── BestDealBadge.jsx
│   ├── services/
│   │   ├── uberService.js
│   │   ├── olaService.js
│   │   └── supabase.js
│   └── navigation/
│       └── AppNavigator.jsx
├── App.jsx
├── app.json
└── package.json
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js >= 18
- Expo CLI: `npm install -g expo-cli`
- Expo Go app on your phone (for testing)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/rideradar.git
cd rideradar

# Install dependencies
npm install

# Start the development server
npx expo start
```

---

## 🔑 API Keys Setup

Create a `.env` file in the root of your project:

```env
GOOGLE_PLACES_API_KEY=your_google_places_api_key
UBER_CLIENT_ID=your_uber_client_id
UBER_CLIENT_SECRET=your_uber_client_secret
UBER_SERVER_TOKEN=your_uber_server_token
OLA_API_KEY=your_ola_api_key
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

> ⚠️ Never commit your `.env` file. It is already included in `.gitignore`.

---

## 🔐 How to Get API Keys

### Google Places API
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project
3. Enable **Places API** and **Maps SDK for Android/iOS**
4. Generate an API key under **Credentials**

### Uber API
1. Go to [Uber Developer Dashboard](https://developer.uber.com)
2. Create a new app
3. Under **Auth**, copy your **Client ID**, **Client Secret**, and **Server Token**
4. Enable the **Ride Request** and **Price Estimates** scopes

### Ola API
1. Go to [Ola Developer Portal](https://devportal.olacabs.com)
2. Register as a developer and create an app
3. Copy your **API Key** from the dashboard
4. Request access to the **Ride Estimate** endpoint

### Supabase
1. Go to [Supabase](https://supabase.com) and create a free project
2. Copy the **Project URL** and **anon public key** from **Project Settings → API**

---

## 🗄️ Database Schema (Supabase)

Run this SQL in your Supabase SQL editor:

```sql
-- Search history
create table searches (
  id uuid default gen_random_uuid() primary key,
  pickup_name text not null,
  pickup_lat float not null,
  pickup_lng float not null,
  drop_name text not null,
  drop_lat float not null,
  drop_lng float not null,
  created_at timestamp default now()
);

-- Fare snapshots
create table fare_snapshots (
  id uuid default gen_random_uuid() primary key,
  search_id uuid references searches(id),
  platform text not null,       -- 'uber' or 'ola'
  cab_type text not null,        -- 'mini', 'sedan', 'suv', 'auto'
  estimated_fare float not null,
  surge_multiplier float default 1.0,
  eta_minutes int,
  created_at timestamp default now()
);
```

---

## 📲 Deep Link Configuration

The app uses deep links to open Uber and Ola directly:

| Platform | Deep Link Format |
|---|---|
| Uber | `uber://?action=setPickup&...` |
| Ola | `olacabs://?pickup=...&drop=...` |

Fallback: If the app is not installed, opens the Play Store / App Store listing.

---

## 🧪 Running Tests

```bash
npm test
```

---

## 📦 Building for Production

```bash
# Android
eas build --platform android

# iOS
eas build --platform ios
```

> Requires [Expo EAS CLI](https://docs.expo.dev/build/introduction/): `npm install -g eas-cli`

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

---

## ⚠️ Disclaimer

RideRadar displays fare **estimates** based on data returned by Uber and Ola APIs. Actual fares may vary due to real-time traffic, surge pricing, or route changes. Always confirm the final fare inside the respective app before booking.

---

## 📄 License

MIT License © 2026 RideRadar
