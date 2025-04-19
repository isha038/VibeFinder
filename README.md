
# 🎵 VibeFinder

**VibeFinder** is a music artist recommendation system that lets users enter an artist’s name and instantly get back a curated list of the top 10 similar artists. It uses:

- **Precomputed embeddings** trained on Last.fm tag data via a local Word2Vec model  
- **Google Firestore** to store artist metadata, embeddings, and precomputed recommendations  
- **Next.js (TypeScript)** as a server‑side API + React front‑end  
- **Last.fm API** for fetching new‐artist tags on the fly (optional extension)  


## 🚀 Features

- **Instant recommendations** for any artist in the database   
- **Secure server‑side data access** via Next.js API routes 
- **Easy local prototype** using Python scripts and Firestore  
- **Pluggable architecture** ready to swap out Python → SageMaker, Firestore → DynamoDB, Next.js → Lambda/API Gateway  


## ⚙️ Prerequisites

- **Node.js** ≥ 14 & **npm**  
- **Python** ≥ 3.7 & **pip**  
- A **Firebase** project with **Firestore** enabled  
- A **Firebase service‑account key** JSON file  
- A **Last.fm API key** (for initial data fetch)  



## Installation

1. Clone this repo:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. Install Python deps:
   ```bash
   pip install requests gensim firebase-admin numpy scikit-learn python-dotenv
   ```

3. Install Node deps:
   ```bash
   cd vibe-finder
   npm install
   cd ..
   ```



## 🔧 Setup

1. **Root `.env`** 
   ```ini
   LASTFM_API_KEY=your_lastfm_api_key
   ```

2. **Frontend env** (in `vibe-finder/.env.local`):
   ```ini
   FIREBASE_PROJECT_ID=your-firebase-project-id
   FIREBASE_CLIENT_EMAIL=your-service-account-email
   FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n…\n-----END PRIVATE KEY-----\n"
   ```

3. Verify your Firestore rules allow server‑side writes (test mode or locked to your SDK).


## 🛠️ Data Pipeline (One‑Time)

1. **Fetch artists & tags**:
   ```bash
   python DataFetching/fetch_artists_tags.py
   ```
   Produces `DataFetching/artist_tags_5000.json`.

2. **Train embeddings & upload to Firestore**:
   ```bash
   python DataFetching/embedding.py
   python DataFetching/firestore.py
   ```

3. **Precompute top 10 similar artists**:
   ```bash
   python DataFetching/precompute_similar.py
   ```
   Adds `similar_artists` to each Firestore doc.



## Running the App

1. Start Next.js in the `vibe-finder` folder:
   ```bash
   cd vibe-finder
   npm run dev
   ```
2. Open <http://localhost:3000>  
3. Enter an artist (e.g. **Dayglow**) and see your precomputed top 10.

---

## 📂 File Structure

```
RootProject/
├── DataFetching/               # Python data pipeline
│   ├── fetch_artists_tags.py
│   ├── artist_tags_5000.json
│   ├── embedding.py
│   ├── serviceAccountKey.json
│   ├── firestore.py
│   └── precompute_similar.py
├── vibe-finder/                # Next.js (TypeScript) app
│   ├── pages/
│   │   ├── api/recommend.ts
│   │   └── index.tsx
│   ├── lib/firebaseAdmin.ts
│   ├── .env.local
│   └── package.json
├── .env                        # Last.fm API key
├── .gitignore
└── README.md
```

