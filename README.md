# NovaMoney — Personal Finance Companion

A production-quality personal finance companion app built with Flutter.
Clean, minimal, offline-first, and designed for everyday use.

**Repository:** https://github.com/Engineerrobin7/NovaMoney

---

## Overview

NovaMoney helps users understand their daily money habits through a simple, engaging mobile experience. It is not a banking app — it is a lightweight companion for tracking transactions, visualising spending patterns, monitoring a no-spend streak goal, and receiving smart insights.

The design is inspired by the calm, minimal aesthetic of apps like Notion and CRED. Every screen is built to feel personal and easy to use from the very first launch.

---

## Screenshots & Features

### 1. Home Dashboard
- Current balance (income − expenses) displayed prominently
- Income and expense summary cards
- Savings rate progress bar with contextual feedback
- Weekly spending bar chart (fl_chart)
- Smart insight banner ("You spent more this week than last week")
- Recent transactions list
- Dark mode toggle

### 2. Transaction Management
- Add / Edit / Delete transactions
- Fields: title, amount, type (income/expense), category, date, note
- Swipe-to-delete with confirmation dialog
- Search bar with live filtering
- Filter sheet by type and category
- Empty states and loading states handled

### 3. Goals — No-Spend Streak System
- Tracks consecutive days with zero expenses
- Animated streak counter with emoji feedback
- 21-day visual calendar (green = no spend, red = had expenses)
- Badge system: 🌱 First Step → ⚡ Momentum → 🔥 On Fire → 💎 Disciplined → 🏆 Legend
- Financial health bars (savings rate, budget used)

### 4. Insights Screen
- Pie chart with category-wise expense breakdown (interactive, touch to reveal %)
- Weekly comparison bar chart (this week vs last week)
- Smart insight cards (top spending category, savings rate, week-over-week change)
- Category breakdown list with progress bars

---

## Tech Stack

| Layer | Choice | Reason |
|---|---|---|
| Framework | Flutter (Dart) | Cross-platform, smooth animations, rich widget ecosystem |
| State Management | Provider | Lightweight, well-suited for this app's complexity |
| Local Database | Hive | Fast, offline-first, no native dependencies |
| Charts | fl_chart | Flexible, beautiful charts for Flutter |
| Date/Currency | intl | Standard formatting |
| IDs | uuid | Collision-free transaction IDs |

---

## Architecture

```
lib/
├── core/
│   ├── constants/      # Categories, icons, box names
│   ├── theme/          # Light & dark ThemeData (AppTheme)
│   └── utils/          # Date & currency formatters
├── models/
│   └── transaction_model.dart   # Hive model + pre-generated adapter
├── services/
│   └── transaction_service.dart # Async CRUD layer (simulates API calls)
├── providers/
│   ├── transaction_provider.dart  # All business logic & derived state
│   └── theme_provider.dart        # Dark mode persistence
├── screens/
│   ├── home/           # Dashboard
│   ├── transactions/   # List screen + Add/Edit form
│   ├── insights/       # Charts & analytics
│   └── goals/          # Streak system & badges
├── widgets/            # AppCard, TransactionTile, EmptyState, SummaryCard, LoadingWidget
└── main.dart           # Hive init, mock seed, MultiProvider, MaterialApp
```

**Separation of concerns:**
- `TransactionService` owns all Hive I/O and simulates async API delays
- `TransactionProvider` owns all derived state (balance, streak, insights, filters)
- Screens are purely reactive — they only call provider methods and render state
- Widgets are fully reusable and receive data via constructor

---

## Design Decisions & Assumptions

- **Offline-first is the primary requirement.** No network dependency. All data lives in Hive on-device.
- **Mock data is seeded on first launch** so the app is immediately useful to evaluate without manual data entry (14 realistic transactions across categories).
- **No-spend streak** counts from today backwards — any day with zero expense entries counts. This is the most intuitive interpretation for a daily habit tracker.
- **Savings rate** = (income − expenses) / income, clamped to [0, 1]. Displayed as a progress bar with contextual copy.
- **The Hive adapter** (`transaction_model.g.dart`) is pre-generated and committed so no `build_runner` step is needed to run the app.
- **Currency** defaults to USD (`$`). Multi-currency is not implemented but the formatter is isolated in one place for easy extension.

---

## Setup & Running

### Prerequisites
- Flutter SDK (stable channel, 3.x+)
- Android emulator / physical device, or iOS simulator

### Steps

```bash
# 1. Clone the repo
git clone https://github.com/Engineerrobin7/NovaMoney.git
cd NovaMoney

# 2. Install dependencies
flutter pub get

# 3. Run the app
flutter run
```

No code generation needed — the Hive adapter is pre-committed.

### Build APK

```bash
flutter build apk --release
```

---

## Optional Node.js Backend

A real REST API is included under `backend/` for bonus points.

### Stack
- Node.js + Express
- MongoDB (Mongoose)
- CORS enabled, JSON responses

### API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | /transactions | List all (supports `?type=`, `?category=`, `?search=`) |
| POST | /transactions | Create transaction |
| PUT | /transactions/:id | Update transaction |
| DELETE | /transactions/:id | Delete transaction |
| GET | /health | Health check |

### Run Backend

```bash
cd backend
npm install

# Edit .env if needed (default: mongodb://localhost:27017/finance_companion)
npm start          # production
npm run dev        # with nodemon auto-reload
```

---

## Evaluation Criteria Mapping

| Criterion | Where to look |
|---|---|
| Product Thinking | Home dashboard layout, insight banner, streak system design |
| Mobile UI/UX | Navigation, empty states, swipe-to-delete, form validation, loading states |
| Creativity | No-spend streak + badge system (Goals screen) |
| Functionality | All CRUD works, search/filter, charts update reactively |
| Code Quality | `flutter analyze lib/` → No issues found |
| State & Data | Provider + Hive, service layer with simulated async, reactive UI |
| Responsiveness | SingleChildScrollView, Expanded/Flexible layouts, tested on multiple sizes |
| Documentation | This README |

---

## What I Would Add With More Time

- Push notifications for streak reminders
- Animated number counters on the dashboard
- CSV/PDF export of transactions
- Biometric lock screen
- Multi-currency support
- Recurring transaction templates
