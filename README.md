# Flashcards - Learning Application with Flash Cards

A flashcard learning application consisting of an iOS app built with SwiftUI and a REST API built with FastAPI.

## 📋 Description

Flashcards is an interactive learning application that uses flash cards. The app allows users to view cards with questions and answers, track their progress, and monitor learning results.

### Key Features

- 🎯 **Interactive Cards**: View questions and answers with the ability to flip cards
- 📊 **Progress Tracking**: Save statistics for each card (know/don't know)
- 👤 **Personalization**: Login with username and save results
- 📈 **Statistics**: View results after completing a session
- 🔄 **Reusability**: Ability to start a new learning session

## 🏗️ Project Architecture

The project consists of two main components:

1. **iOS Application** (`FLASHCARDS_NFACT_IOS/flashcards/`) - Client application built with SwiftUI
2. **Backend API** (`FLASHCARDS_NFACT_IOS/flashcards_api/`) - REST API built with FastAPI and PostgreSQL

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- PostgreSQL (can use Docker)
- Xcode 12+ (for iOS application)
- Docker (optional, for easier database setup)

### Backend API Installation and Setup

1. **Navigate to the API directory:**
```bash
cd FLASHCARDS_NFACT_IOS/flashcards_api
```

2. **Create a virtual environment (recommended):**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Configure the database:**

   Create a `.env` file in the `flashcards_api/` directory:
   ```env
   DATABASE_URL=postgresql://postgres:postgres@localhost:5433/flashcards
   ```

   Run PostgreSQL via Docker:
   ```bash
   docker run --name flashcards-db -e POSTGRES_PASSWORD=postgres -p 5433:5432 -d postgres
   ```

   Or use an existing PostgreSQL database.

5. **Start the FastAPI server:**
```bash
uvicorn app.main:app --reload
```

6. **Load cards:**

   - Open Swagger documentation: http://127.0.0.1:8000/docs
   - Find the `POST /load_cards` endpoint
   - Click "Try it out" → "Execute"
   - Cards will be loaded from Open Trivia Database

### Running the iOS Application

1. **Open the project in Xcode:**
```bash
cd FLASHCARDS_NFACT_IOS/flashcards/flashcards/flashcards.xcodeproj
open flashcards.xcodeproj
```

2. **Configure API URL (if needed):**

   In the `FlashcardService.swift` file, change `baseURL` if your API runs on a different address:
   ```swift
   private let baseURL = "http://127.0.0.1:8000"
   ```

   **Important:** For real iOS devices, use your computer's IP address instead of `127.0.0.1`.

3. **Run the application:**

   - Select a simulator or connected device
   - Press Run (⌘R)

## 📱 Using the Application

1. **Start:** Enter your name on first launch
2. **Study:** View cards, press "Flip" to see the answer
3. **Evaluate:** Press "Know" or "Don't Know" for each card
4. **Results:** After going through all cards, view statistics
5. **Repeat:** Press "Play Again" for a new session

## 🔌 API Endpoints

### `GET /`
Health check for the API
- **Response:** `{"message": "Flashcards API is running"}`

### `GET /cards`
Get all cards
- **Response:** List of cards in JSON format

### `POST /load_cards`
Load cards from Open Trivia Database
- **Response:** `{"message": "Cards loaded successfully"}`

### `POST /progress`
Create a user progress record
- **Request Body:**
  ```json
  {
    "user_id": "string",
    "card_id": 1,
    "status": "know" | "dont_know"
  }
  ```

### `GET /progress?user_id={user_id}`
Get user progress
- **Parameters:** `user_id` (query parameter)
- **Response:** List of progress records

## 📂 Project Structure

```
flashcards-1/
├── FLASHCARDS_NFACT_IOS/
│   ├── flashcards/                    # iOS application
│   │   └── flashcards/
│   │       └── flashcards/
│   │           ├── ContentView.swift      # Main view
│   │           ├── StartView.swift        # Login screen
│   │           ├── FlashcardsView.swift   # Cards screen
│   │           ├── ResultView.swift       # Results screen
│   │           ├── FlashcardService.swift # API service
│   │           ├── ProgressService.swift  # Progress sending service
│   │           └── UserDefaultsService.swift # Local storage
│   │
│   └── flashcards_api/               # Backend API
│       └── app/
│           ├── main.py                # FastAPI application
│           ├── models.py              # SQLAlchemy models
│           ├── schemas.py             # Pydantic schemas
│           ├── database.py            # Database configuration
│           └── utils.py               # Utilities (card loading)
│       ├── requirements.txt           # Python dependencies
│       └── README.md                  # API documentation
└── README.md                          # This file
```

## 🛠️ Technologies

### Backend
- **FastAPI** - Modern web framework for Python
- **SQLAlchemy** - ORM for database operations
- **PostgreSQL** - Relational database
- **httpx** - Async HTTP client
- **Pydantic** - Data validation

### iOS
- **SwiftUI** - Framework for building user interfaces
- **Foundation** - Network and data handling
- **UserDefaults** - Local storage

## 📝 Data Models

### Card
- `id` - Unique identifier
- `question` - Question text
- `answer` - Answer text
- `topic` - Topic/category

### Progress
- `id` - Unique identifier
- `user_id` - User identifier
- `card_id` - Card identifier
- `status` - Status ("know" or "dont_know")

## 🔧 Configuration

### Changing Card Source

By default, cards are loaded from Open Trivia Database. To change the source, edit the `load_cards_from_opentdb` function in `app/utils.py`.

### Database Configuration

Change the `DATABASE_URL` variable in the `.env` file to connect to a different database.

### API Port Configuration

Modify the startup command:
```bash
uvicorn app.main:app --reload --port 8000
```

## 🐛 Troubleshooting

### iOS App Cannot Connect to API

- Ensure the API is running and accessible
- For simulator, use `127.0.0.1` or `localhost`
- For real device, use your computer's IP address on the local network
- Check that port 8000 is not blocked by firewall

### Database Connection Error

- Ensure PostgreSQL is running
- Verify `DATABASE_URL` in the `.env` file is correct
- Ensure port 5433 (or other specified port) is accessible

### Cards Not Loading

- Check internet connection (for loading from Open Trivia Database)
- Ensure the `/load_cards` endpoint was called successfully
- Check server logs for errors

## 📄 License

This project is created for educational purposes.

## 👤 Author

Ramazan Perdebai

## 🙏 Acknowledgments

- Open Trivia Database for providing the API to fetch questions
- FastAPI for the excellent framework
- SwiftUI for the convenient iOS app development tool
