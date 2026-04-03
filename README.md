# Finance Companion

A production-quality personal finance tracking app built with Flutter.

## Features

- **Home Dashboard** — balance, income/expense summary, savings progress, weekly spending chart, smart insights
- **Transactions** — add/edit/delete, swipe-to-delete, search & filter by type/category
- **Insights** — pie chart by category, weekly comparison, smart insight cards
- **Goals & Streaks** — no-spend streak tracker, 21-day calendar, badges, financial health
- **Dark Mode** — toggle from the home screen
- **Offline-first** — all data stored locally with Hive
- **Mock data** — seeded automatically on first launch

## Project Structure

```
lib/
├── core/
│   ├── constants/    # App-wide constants, categories, icons
│   ├── theme/        # Light & dark ThemeData
│   └── utils/        # Date/currency formatters
├── models/           # TransactionModel (Hive)
├── services/         # TransactionService (simulated async CRUD)
├── providers/        # TransactionProvider, ThemeProvider
├── screens/
│   ├── home/         # Dashboard
│   ├── transactions/ # List + Add/Edit form
│   ├── insights/     # Charts & analytics
│   └── goals/        # Streak system & badges
├── widgets/          # Reusable UI components
└── main.dart
```

## Flutter Setup

```bash
# Install dependencies
flutter pub get

# Run on device/emulator
flutter run

# Build APK
flutter build apk --release
```

> No code generation needed — the Hive adapter (`transaction_model.g.dart`) is pre-generated.

---

## Optional Node.js Backend

### Prerequisites
- Node.js 18+
- MongoDB running locally on port 27017

### Setup

```bash
cd backend
npm install
```

Edit `.env` if your MongoDB URI differs:
```
PORT=3000
MONGO_URI=mongodb://localhost:27017/finance_companion
```

### Run

```bash
# Production
npm start

# Development (with auto-reload)
npm run dev
```

### API Endpoints

| Method | Endpoint              | Description          |
|--------|-----------------------|----------------------|
| GET    | /transactions         | List all (supports ?type=, ?category=, ?search=) |
| POST   | /transactions         | Create transaction   |
| PUT    | /transactions/:id     | Update transaction   |
| DELETE | /transactions/:id     | Delete transaction   |
| GET    | /health               | Health check         |

### Example POST body

```json
{
  "title": "Grocery Store",
  "amount": 85.50,
  "type": "expense",
  "category": "Food",
  "date": "2026-04-03T00:00:00.000Z",
  "note": "Weekly groceries"
}
```
