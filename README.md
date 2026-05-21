# TripPlanner ✈️

A feature-rich, modern Android travel assistant designed to simplify trip planning, budget management, localized weather tracking, automated packing lists, and AI-powered interactive itinerary planning.

---

## 🌟 Key Features

### 1. Dynamic Destination & POI Discovery (`AttractionsFragment`)
- **Smart Discovery**: Fetches top points-of-interest (Sights, Food, Nightlife, Stays, Beaches, Parks) tailored to the destination.
- **Biased Search**: Resolves search coordinates via Open-Meteo Geocoding and filters local hotspots within a 10km radius using the **Google Places API (New)**.
- **Interactive Map Routing**: Direct deep-linking navigation to Google Maps.
- **Favorites & Star System**: Star custom POIs to pin them to the top of the feed.
- **Seen Filters**: Mark places as "Visited" to filter them out of active discovery.
- **Smart Offline Fallback**: Features high-fidelity pre-compiled demo data for testing when offline.

### 2. Conversational AI Itinerary Chatbot (`ChatbotFragment`)
- **Conversational UX**: Chat interface matching the app's premium dark mode theme, featuring HTML bolding, formatted paragraph spacing, and emoji markers.
- **Gemini REST Engine**: Utilizes raw asynchronous HTTP requests via **OkHttp** to target the **Gemini 3.5 Flash** REST endpoint (`v1/models/gemini-3.5-flash`), bypassing complex SDK setups.
- **Structured Outputs**: Prompts the AI model to return structured day-by-day JSON plans.

### 3. Interactive Checklists & Progress Tracker (`ItineraryDetailDialogFragment`)
- **Interactive Checklists**: Double-tapping or clicking anywhere on an itinerary card toggles a checkmark.
- **Strike-through & Opacity**: Checked activities dynamically fade and get crossed out.
- **Progress Tracking**: Real-time progress count (e.g., `3 / 12 completed`) coupled with a matching **LinearProgressIndicator**.
- **Day Grouping**: Dynamically groups plans by days with clean teal divider headers.
- **Persistent States**: Checkbox selections are written back to JSON and stored directly in the local SQLite database.

### 4. Intellect-driven Packing List Generator (`PackListFragment`)
- **Eco-Classification**: Geocodes the destination and runs semantic analysis to tag destination types (Beach, Mountain, Desert, Cold, City, Religious).
- **Weather Integration**: Cross-references weather forecast telemetry to check for rain warnings, extreme heat, cold drops, and wind advisory codes.
- **Custom-tailored Items**: Merges destination tag, weather flags, and user-selected activities (hiking, shopping, camping, food-touring) to generate a personalized checkbox packing list.

### 5. Multi-category Budget & Expense Manager (`BudgetActivity`)
- **Total vs Spent Analysis**: Track remaining budget in real time with red highlighting for overspent budgets and teal for positive balances.
- **Removable Item Chips**: Add expenses (Accommodation, Food, Transport, Activities) represented as clean closeable Material Chips.
- **Legacy Compatibility**: Automatically migrates old single-value keys to chip lists on startup.

### 6. Weather & Climate Forecast (`WeatherFragment`)
- **14-day Weather Dashboard**: Details maximum/minimum temperatures, humidity, wind speeds, and precipitation levels via keyless **Open-Meteo** API integration.
- **Weather Alerts**: Provides smart warning chips if rain is forecast during the trip dates.

---

## 🛠️ Architecture & Tech Stack

- **Android Jetpack**: Fragment-based page navigation, DialogFragments, and RecyclerView adapters.
- **HTTP client**: Asynchronous parallel requests handled via **OkHttp 4.12**.
- **JSON Parsing**: Structured serialization/deserialization with `org.json`.
- **Image Loader**: **Glide 4.16** with OkHttp integration handles Places media redirects and fades images into views smoothly.
- **Database**: Local relational storage handled via **SQLite** (`DatabaseHelper.java`), storing trip details and itinerary configurations.
- **Session State**: **SharedPreferences** caches current trip IDs, auth state, and active trip telemetry.

---

## 🔑 Secure API Configuration & Firebase Setup

To keep production credentials secure, this repository uses a dual-method configuration strategy separating runtime user keys from central build-time services.

### 1. Firebase Service Setup (Build-Time)
The app uses Firebase Authentication to manage user signups and logins. To prevent credential leaks, the actual configuration is kept out of Git.

1. **Automatic Template Fallback**: A template `app/google-services.json.template` is provided. If `app/google-services.json` is missing, the Gradle build will automatically copy the template so the project builds out of the box.
2. **Adding Your Production Firebase Config**:
   - Go to the [Firebase Console](https://console.firebase.google.com/).
   - Create a project and add an Android app with package name `com.example.tripplanner`.
   - Download the generated `google-services.json` configuration file.
   - Place it inside the `app/` folder (overwriting the placeholder/generated `app/google-services.json`).
   - The file `app/google-services.json` is ignored in `.gitignore` to guarantee your API keys are never checked into version control.

### 2. Google Places & Gemini API Keys (Runtime)
No Google Cloud or Gemini keys are hardcoded or required at build time. These are entered directly in the app at runtime:

1. Build and run the project in Android Studio.
2. Sign up or log in (Firebase will run in offline demo mode if dummy keys are used, or authenticate online if real ones are provided).
3. Tap **My Profile** on the Dashboard.
4. Input your **Google Places API Key** and **Google Gemini API Key**, and click **Save**.
5. The keys are encrypted/secured in your device's local private `SharedPreferences`.

---


## 📂 Core Package Structure

```
com.example.tripplanner
├── MainActivity.java               # Houses the main bottom navigation view pager
├── DashboardActivity.java          # Landing screen containing trip summaries & statistics
├── DatabaseHelper.java             # SQLite tables for Trips, Itineraries, and Packing items
├── LoginActivity.java              # Directs login sessions and verification
├── SignupActivity.java             # Persists local profile registration
├── ProfileActivity.java            # Configures user profile details and API keys
├── AttractionsFragment.java        # POI Feed powered by Google Places API (New)
├── WeatherFragment.java            # Climate telemetry via Open-Meteo
├── ChatbotFragment.java            # DialogFragment handling Gemini REST queries
├── ItineraryFragment.java          # List showing all planned itineraries
├── ItineraryDetailDialogFragment.java # Checkable task view for active itineraries
├── PackListFragment.java           # Weather and activity-informed packing checklist
└── BudgetActivity.java             # Custom material budget manager
```
