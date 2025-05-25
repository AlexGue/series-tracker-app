# Series Aggregator Project

A web application that allows users to track and manage their TV series watching progress without requiring account registration. Users can search for series, filter by various criteria, mark episodes as viewed, and maintain watchlists - all while maintaining their data through a unique URL-based session system.

## Project Purpose

The Series Aggregator is a web application that allows users to track and manage their TV series watching progress without requiring account registration. Users can search for series, filter by various criteria, mark episodes as viewed, and maintain watchlists - all while maintaining their data through a unique URL-based session system.

## Core Requirements

### User Session Management
- No user registration required - anonymous access
- Generate random UUID when user first interacts with the page and add it to URL
- UUID serves as unique identifier for user data storage
- Users can bookmark/share their personalized URL to access their data

### Series Search & Filtering
- Search functionality for finding series by title
- Filter by Last Updated Year (latest season/episode release)
- Filter by Platforms (Netflix, HBO Max, Disney+, Amazon Prime, Hulu, etc.)
- Filter by Category/Genre (Drama, Comedy, Sci-Fi, Horror, etc.)
- Multiple filters can be applied simultaneously

### Series Tracking
- Mark series as 'Viewed' - tracks current episode watched
- When marking as viewed, store current season and episode numbers
- Ability to update viewing progress (move to next/previous episode)
- Visual indicators showing watch progress (e.g., season 2 episode 5 of 10)

### Watchlist Management
- Add series to 'Pending to Watch' list
- Remove series from pending list
- Move series from pending to currently watching
- View separate lists for: Currently Watching, Completed, Pending

## Data Model

### User Entity
```json
{
  "id": "uuid", // Primary key - UUID generated on first visit
  "createdAt": "timestamp", // When user first accessed the site
  "lastAccessedAt": "timestamp", // Last activity timestamp
  "preferences": { // Optional user preferences
    "theme": "dark|light",
    "defaultView": "grid|list"
  }
}
```

### Series Entity
```json
{
  "id": "string", // Unique identifier (could be TMDB ID or internal)
  "title": "string", // Series title
  "description": "string", // Series synopsis
  "posterUrl": "string", // URL to poster image
  "genres": ["string"], // Array of genres
  "platforms": ["string"], // Available streaming platforms
  "totalSeasons": "number",
  "status": "ongoing|completed|cancelled",
  "firstAired": "date",
  "lastUpdated": "date", // Last episode/season release
  "imdbRating": "number",
  "seasons": [
    {
      "seasonNumber": "number",
      "episodeCount": "number",
      "aired": "date"
    }
  ]
}
```

### UserSeriesTracking Entity
```json
{
  "id": "string", // Primary key
  "userId": "string", // Foreign key to User
  "seriesId": "string", // Foreign key to Series
  "status": "watching|completed|pending|dropped",
  "currentSeason": "number", // Current season being watched
  "currentEpisode": "number", // Current episode in season
  "rating": "number", // User's personal rating (1-10, optional)
  "notes": "string", // Personal notes (optional)
  "dateAdded": "timestamp", // When added to list
  "dateStarted": "timestamp", // When started watching (optional)
  "dateCompleted": "timestamp", // When completed (optional)
  "lastUpdated": "timestamp", // Last progress update
  "lastViewedDate": "timestamp", // Date when user last marked an episode as viewed
}
```

### Platform Entity
```json
{
  "id": "string", // Primary key
  "name": "string", // Platform name (Netflix, HBO Max, Disney+, etc.)
  "slug": "string", // URL-friendly identifier (netflix, hbo-max, disney-plus)
  "logoUrl": "string", // Platform logo image URL
  "iconUrl": "string", // Small icon version URL
  "color": "string", // Brand color (hex code)
  "website": "string", // Official platform website
  "isActive": "boolean", // Whether platform is still active
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

## Technical Considerations

### Data Source
- Integrate with TMDB (The Movie Database) API for series information
- Alternative: JustWatch API for platform availability data
- Scheduled data refresh to keep platform availability current

### Core API Endpoints
- GET /api/series/search?q={query}&platform={platform}&genre={genre}&year={year}
- GET /api/user/{userId}/tracking - Get user's tracked series
- POST /api/user/{userId}/tracking - Add series to tracking
- PUT /api/user/{userId}/tracking/{seriesId} - Update watching progress
- DELETE /api/user/{userId}/tracking/{seriesId} - Remove from tracking

### Technology Stack Recommendations
- Frontend: React with TypeScript and Tailwind CSS for styling
- Backend: Node.js with Express.js for REST API
- Database: MongoDB with Mongoose for data modeling
- External APIs: TMDB API for series data and metadata
- State Management: React Context API with useReducer
- HTTP Client: Axios for API communication
- Development: Vite for fast development and build tooling

## Security Considerations
- URL IDs should be cryptographically secure and non-guessable
- Email verification required before sending notifications
- Rate limiting on API endpoints
- Data sanitization for all user inputs

## Deployment Strategy
- Containerized application with Docker
- MongoDB Atlas for database
- Vercel/Netlify for frontend hosting
- Node.js server deployment on Heroku/DigitalOcean
- Scheduled tasks via cron jobs or dedicated worker service

## Next Steps for Development

### Phase 1: Project Setup & API Integration (Week 1-2)
- Create TMDB API account and explore TV series endpoints
- Set up project structure: React frontend + Node.js backend
- Configure MongoDB database (local development + MongoDB Atlas)
- Create Mongoose schemas for User, Series, Platform, UserSeriesTracking
- Implement basic TMDB API integration and data fetching
- Set up Vite + React + TypeScript + Tailwind CSS frontend

### Phase 2: Core User System (Week 3-4)
- Implement UUID generation and URL parameter handling
- Create user auto-creation on first visit logic
- Build React Context for user state management
- Implement persistent URL-based sessions
- Create basic navigation and layout components

### Phase 3: Series Search & Discovery (Week 5-6)
- Build series search API endpoints with TMDB integration
- Create search UI with filters (year, platform, genre)
- Implement series cards/grid layout with posters
- Add platform logos and visual indicators
- Create platform management system and seed data
- Implement infinite scroll or pagination for search results

### Phase 4: Series Tracking System (Week 7-8)
- Build API endpoints for tracking operations (CRUD)
- Create series detail view with episode tracking
- Implement 'Add to Pending', 'Mark as Watching', status changes
- Build episode progress tracking with season/episode selectors
- Add lastViewedDate tracking for special/random episodes
- Create user dashboard with tracking lists (Watching, Completed, Pending)

### Phase 5: UI/UX Enhancement & Responsive Design (Week 9-10)
- Implement responsive design for mobile and tablet devices
- Add loading states, skeleton screens, and error handling
- Implement dark/light theme support
- Add smooth animations and transitions
- Create comprehensive search filters and sorting options
- Implement progress visualization (progress bars, completion percentages)

### Phase 6: Testing, Optimization & Deployment (Week 11-12)
- Write unit tests for critical backend functionality
- Add integration tests for API endpoints
- Implement error logging and monitoring
- Optimize database queries and add proper indexing
- Set up production deployment (backend + database)
- Deploy frontend to Vercel/Netlify with environment configuration
- Conduct end-to-end testing and bug fixes

---

## 🚀 Project Status & Progress Tracking

**Current Phase**: Phase 1 - Project Setup & API Integration
**Status**: 🟡 IN PROGRESS
**Last Updated**: May 25, 2025

### Phase 1 Progress (Week 1-2)

- ✅ **COMPLETED**: Create TMDB API account and explore TV series endpoints
- 🟡 **IN PROGRESS**: Set up project structure: React frontend + Node.js backend
- ⚪ **TODO**: Configure MongoDB database (local development + MongoDB Atlas)
- ⚪ **TODO**: Create Mongoose schemas for User, Series, Platform, UserSeriesTracking
- ⚪ **TODO**: Implement basic TMDB API integration and data fetching
- ⚪ **TODO**: Set up Vite + React + TypeScript + Tailwind CSS frontend

### 📋 Current Work Details

**TMDB API Setup - COMPLETED ✅**
- Created TMDB account: https://www.themoviedb.org/
- API Key obtained (store in environment variables)
- Key endpoints identified:
  • Search TV: `/search/tv?api_key={key}&query={query}`
  • TV Details: `/tv/{tv_id}?api_key={key}`
  • TV Season: `/tv/{tv_id}/season/{season_number}?api_key={key}`
  • Discover TV: `/discover/tv?api_key={key}&with_genres={genre_id}`
  • Watch Providers: `/tv/{tv_id}/watch/providers?api_key={key}`

**Project Structure Setup - IN PROGRESS 🟡**
- Project folder structure created:
```
series-aggregator/
├── backend/          # Node.js + Express API
│   ├── src/
│   │   ├── models/   # Mongoose schemas
│   │   ├── routes/   # API endpoints
│   │   ├── services/ # Business logic
│   │   └── utils/    # Helper functions
│   ├── package.json
│   └── .env.example
└── frontend/         # React + Vite application
    ├── src/
    │   ├── components/ # React components
    │   ├── pages/      # Page components
    │   ├── contexts/   # React contexts
    │   ├── services/   # API calls
    │   └── utils/      # Helper functions
    ├── package.json
    └── .env.example
```

### 🚧 Next Actions Required

**IMMEDIATE PRIORITY:**
- 1. Initialize backend Node.js project with Express
- 2. Initialize frontend React project with Vite + TypeScript
- 3. Set up MongoDB connection (local + cloud)

**COMMANDS TO RUN:**
```bash
# Backend setup
mkdir series-aggregator && cd series-aggregator
mkdir backend && cd backend
npm init -y
npm install express mongoose cors dotenv axios
npm install -D nodemon

# Frontend setup
cd ../frontend
npm create vite@latest . -- --template react-ts
npm install
npm install -D tailwindcss postcss autoprefixer
npm install axios react-router-dom
```

### 🔑 Environment Variables Required

**Backend (.env):**
```
PORT=5000
MONGODB_LOCAL=mongodb://localhost:27017/series-aggregator
MONGODB_ATLAS=mongodb+srv://<username>:<password>@cluster.mongodb.net/series-aggregator
TMDB_API_KEY=your_tmdb_api_key_here
TMDB_BASE_URL=https://api.themoviedb.org/3
NODE_ENV=development
```

**Frontend (.env):**
```
VITE_API_BASE_URL=http://localhost:5000/api
```

### 📦 Handoff Notes for Next Developer

**Current Status**: Phase 1 is 20% complete. TMDB API setup is finished, project structure planned.

**What's Ready:**
- ✅ TMDB API account created with working credentials
- ✅ All data models documented (User, Series, Platform, UserSeriesTracking)
- ✅ Project folder structure defined
- ✅ Technology stack confirmed: MongoDB + React + Node.js + Tailwind

**Next Developer Should:**
- 1. Run the setup commands above to initialize both projects
- 2. Set up MongoDB Atlas account and create cluster
- 3. Create the Mongoose schemas based on the data models above
- 4. Test TMDB API integration with a simple search endpoint

**Expected Timeline**: Complete Phase 1 within 1-2 weeks, then move to Phase 2 (User System).

📝 **Remember to update this status section as work progresses!**
