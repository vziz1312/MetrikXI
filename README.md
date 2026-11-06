# MetrikXI

### Clarity for every scouting decision.

MetrikXI is a football scouting and player analytics platform in development, designed to help identify, evaluate and compare football players using performance data and statistical similarity.

The project combines Python-based scouting models, a SQLite database and a FastAPI backend. An Angular frontend is planned to provide a professional, data-driven recruitment workspace.

---

## Overview

MetrikXI aims to make football recruitment analysis more structured and accessible.

The platform is being developed around six core modules:

- **Discover** — Search and filter football players.
- **Player Profiles** — Explore individual player information and performance statistics.
- **Similar Players** — Discover statistically comparable players.
- **Compare** — Analyse multiple players side by side.
- **Shortlists** — Organise potential recruitment targets.
- **Reports** — Generate structured scouting reports.

## Current Implementation

### Backend API

The backend is developed with **FastAPI** and connected to a local SQLite database.

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | API welcome message |
| GET | `/health` | Backend health check |
| GET | `/players/search` | Search players by name |
| GET | `/players/{player_id}` | Retrieve a player profile |
| GET | `/players/{player_id}/similar` | Retrieve similar-player recommendations |

The API supports accent-insensitive player searches and returns structured JSON responses.

### Player Similarity Model

The current scouting model is implemented in `transfermarkt_scouting.py`.

It uses player performance records from Transfermarkt data to calculate statistical similarity.

The current model:

1. Searches for players using normalized names.
2. Aggregates career performance statistics.
3. Requires a minimum of 100 appearances by default.
4. Filters candidates by the target player's main position when available.
5. Represents players using goals per appearance and assists per appearance.
6. Calculates Euclidean distance between player profiles.
7. Converts the distance into a similarity score.

The similarity formula is:

`similarity = 1 / (1 + Euclidean distance)`

A higher score indicates that the two statistical profiles are closer.

**Important:** The current model focuses on attacking output. Its similarity score is a ranking measure, not a probability or a complete assessment of playing style.

### Experimental Hybrid Model

The repository also contains an experimental model:

`transfermarkt_scouting_hybrid.py`

It explores:

- Recent-season performance analysis.
- Career-wide statistical fallback.
- Position-based filtering.
- Sample-size adjustment.
- Accent-insensitive player matching.

This model is maintained separately from the career-based model currently connected to FastAPI.

---

## Technology Stack

| Component | Technology |
|---|---|
| Frontend (planned) | Angular, TypeScript |
| Backend | Python, FastAPI |
| ASGI server | Uvicorn |
| Database | SQLite |
| Data processing | Pandas |
| Automated testing | Pytest |
| API testing | FastAPI TestClient |

## Project Structure

```text
MetrikXI/
│
├── api.py
├── transfermarkt_scouting.py
├── transfermarkt_scouting_hybrid.py
│
├── similarity.py
├── main.py
├── database.py
├── collect_data.py
│
├── import_transfermarkt.py
├── add_appearances.py
│
├── test_api.py
├── test_similarity.py
├── test_transfermarkt_scouting_hybrid.py
│
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

Local SQLite databases and environment files are intentionally excluded from Git.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/vziz1312/MetrikXI.git
cd MetrikXI
```

### 2. Install dependencies

Python 3.10 or newer is recommended.

```bash
python -m pip install -r requirements.txt
```

### 3. Prepare the database

The FastAPI backend requires a local `transfermarkt.db` file beside `api.py`.

The database must contain:

- A `players` table with player profile information.
- A `performances` table containing player performance records.
- Matching `player_id` fields linking both tables.

The similarity model also requires appearance, goals and assists data.

**Note:** The database is not included in the repository. A compatible local dataset must be prepared separately.

### 4. Start the backend

```bash
python -m uvicorn api:app --reload
```

The API runs locally at:

`http://127.0.0.1:8000`

Interactive API documentation:

`http://127.0.0.1:8000/docs`

### 5. Run automated tests

```bash
python -m pytest -v
```

The current test suite includes:

- API endpoint tests.
- Player similarity tests.
- Hybrid scouting model tests.
- SQLite integration tests.

---

## Design Direction

MetrikXI follows an editorial football intelligence design philosophy.

The interface is intended to resemble professional recruitment software rather than a generic analytics dashboard.

### Visual Identity

| Colour | HEX | Usage |
|---|---|---|
| Paper White | `#F3F2ED` | Main background |
| Ink | `#202830` | Primary text |
| Pitch Green | `#536B58` | Main accent |
| Mist | `#D9DED8` | Borders and secondary surfaces |
| Terracotta | `#A86650` | Secondary highlights |
| White | `#FFFFFF` | Content panels |
| Deep Pine | `#142B25` | Sidebar and navigation |
| Soft Sage | `#DCE7D9` | Subtle highlights |

### Design Principles

- Clean editorial layouts.
- Restrained colours and consistent spacing.
- Compact, readable statistical tables.
- Clear player profiles and analytical sections.
- Minimal decorative elements.
- Real data instead of fabricated statistics.
- Visualisations supported by reliable underlying data.

### Tagline

**Clarity for every scouting decision.**

---

## Roadmap

### Backend Development
- [x] FastAPI application setup.
- [x] SQLite database integration.
- [x] Player search endpoint.
- [x] Player profile endpoint.
- [x] Similar-player recommendation endpoint.
- [x] Automated API tests.
- [ ] Expanded player statistics endpoints.
- [ ] Improved API validation and error handling.

### Frontend Development
- [ ] Angular application setup.
- [ ] MetrikXI design system.
- [ ] Navigation and dashboard layout.
- [ ] Player discovery and search interface.
- [ ] Individual player profiles.
- [ ] Similar-player recommendations interface.
- [ ] Player comparison.
- [ ] Shortlists and scouting reports.
- [ ] Statistical charts and visualisations.

### Future Improvements
- [ ] Richer position-specific similarity features.
- [ ] More detailed seasonal performance analysis.
- [ ] Advanced player comparison.
- [ ] Evaluation of recommendations against scouting criteria.
- [ ] Reliable positional data integration.
- [ ] Deployment and production preparation.

---

## Current Limitations

- The current career-based similarity model uses only goals and assists per appearance.
- The recommendation score does not represent overall playing-style similarity.
- Results depend on the completeness and consistency of the available dataset.
- The experimental hybrid model and the API-connected model are separate implementations.
- The Angular frontend has not yet been implemented.
- Positional heatmaps require positional or tracking data that is not currently available.

---

## Author

**Mohamed Aziz Mghirbi**

GitHub: [@vziz1312](https://github.com/vziz1312)

Repository: [github.com/vziz1312/MetrikXI](https://github.com/vziz1312/MetrikXI)

---

*MetrikXI — Football Intelligence. Measured Precisely.*