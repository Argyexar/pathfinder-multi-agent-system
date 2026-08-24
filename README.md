# 🏔️ Pathfinder: The AI Trail Companion
> **"Find your trail. Leave no trace."** — AI-powered sustainable outdoor discovery for Greece.

[![Made at Makeathon 2026](https://img.shields.io/badge/Makeathon-2026-brightgreen)](https://makeathon.uniai.gr)
![Deloitte Challenge](https://img.shields.io/badge/Challenge-SustainableTourism-blue)
![Team](https://img.shields.io/badge/Team-NNTUA-orange)

---
> ΝΟΤΕ: This is a hackathon showcase prototype. Some integrations depend on external API keys or optional cloud resources, and the original submitted repository remains private. The public goal of this repo is to document the idea, demonstrate the architecture, and preserve the product story behind the Makeathon submission.


## 🎯 The Challenge & Problem
Greece boasts over **3,000 documented trails**, yet overtourism concentrates 36M+ tourists into just 5 major regions, leaving hidden gorges, trails, and local communities bypassed. 
* **The Problem:** Tourists spend hours queuing instead of exploring, while local economies outside major hotspots miss out on revenue.
* **The Goal:** Build an AI trail companion that converses with travelers, profiles their fitness/interests, and generates personalized, sustainable trail itineraries across Greece using open data.

---

## 🚀 The Solution: Pathfinder
Pathfinder is more than just an app; it's a **sustainable tourism distribution channel**. It redirects traveler flows away from over-touristed hotspots by matching users with hidden gems based on their personal preferences, real-time safety, weather, and crowd conditions.

### ✨ Key Features Implemented
- **Conversational profiling**: collects trip duration, fitness level, terrain preferences, accessibility needs, origin city, and interests through natural dialogue.
- **Trail discovery**: searches and ranks Greek hiking options using open data, cached knowledge, and web/RAG enrichment.
- **Sustainability scoring**: combines crowding signals, biodiversity value, and local economic benefit into an interpretable 0-100 score.
- **Real-time conditions**: checks weather, safety flags, recent trail reports, and context that may affect the hike.
- **Photo-to-trail matching**: lets users describe or upload a landscape mood and find visually similar Greek trails.
- **Itinerary generation**: turns the selected recommendation into a day-by-day plan with practical guidance.
- **Saved trips and feedback**: stores selected itineraries and interaction events for future personalization.

---
## 🍿 Demo

[![Demo thumbnail](https://github.com/user-attachments/assets/667db5f8-7cea-4450-972d-75a62bb43086)](https://1drv.ms/v/c/20fe3aee5b9a4c5b/IQB8c-3Q-9hlT5-yYUP_X3A_AeTsgsNMOr0-8DiaWeYRIEg?e=Ct1S3h)

---

## 🛠️ Architecture

PathFinder's Makeathon demo is built around Google ADK multi-agent system using an Azure OpenAI / OpenAI-compatible LLM endpoint. A root orchestrator agent manages the conversation and calls specialist agents as tools. Each specialist owns a focused part of the travel-planning workflow.

```text
User
  |
  v
PathFinder Orchestrator
  |  Agents_flash/pathfinder/agent.py
  |
  +--> analyze_photo_terrain()
  |
  +--> Trail Finder Agent
  |     +--> query_trail_database()
  |     +--> match_trails_by_visual_tags()
  |
  +--> Conditions Agent
  |     +--> get_weather_forecast()
  |     +--> flag_unsafe_conditions()
  |     +--> tavily_search_recent_conditions()
  |
  +--> Itinerary Agent
        +--> get_biodiversity_observations()
        +--> get_nearby_amenities()
        +--> calculate_carbon_footprint()
        +--> generate_gpx()
```

The orchestrator asks a few profile questions, detects whether the user is asking for a normal recommendation or a photo-based match, then presents trail options before checking conditions and producing the final itinerary.

---

## 🤖 Main Agent Responsibilities

| Agent | Responsibility |
|---|---|
| PathFinder Orchestrator | Greets the user, collects profile details, handles photo intent, calls the specialist agents, and formats the final answer into `Recommended Trails` and `Detailed Itinerary` sections |
| Trail Finder Agent | Queries curated Greek trail data, handles visual-tag matching, ranks candidates, adds sustainability score breakdowns, coordinates, and OSM map links |
| Conditions Agent | Checks forecast weather, safety flags, and recent web condition reports before the itinerary is finalized |
| Itinerary Agent | Builds the day plan, enriches it with biodiversity, amenities, carbon footprint, local-business suggestions, and GPX export |

---

## 🌐 Data And APIs

PathFinder follows an **open-data-first** approach.

| Source | Used For |
|---|---|
| OpenStreetMap / Overpass | Trails, POIs, nearby amenities, map context |
| OpenRouteService | Hiking routes and distance/elevation-aware routing |
| Open Elevation / NASA SRTM-style data | Elevation profiles |
| OpenWeatherMap / Open-Meteo | Weather and forecast context |
| iNaturalist | Wildlife observations and biodiversity signals |
| Tavily Search | Recent trail reports, safety updates, and web context |
| ChromaDB / Azure AI Search | Local or cloud vector search for trail knowledge |
| Cosmos DB / local fallback | Saved itineraries, users, and interaction history |

The implementation is intentionally fallback-friendly: local ChromaDB and local history/storage modes make development possible without provisioning the full cloud stack.

---

## 🔮 Future Directions

- Predictive crowd forecasting by season and region.
- Accommodation and local-business booking layer.
- Municipality-facing dashboard for demand insights and infrastructure planning.
- Clear sponsored recommendation layer with transparency controls.
- Stronger feedback loop for personalization and recommendation learning.
- More robust accessibility filters and verified route metadata.

---

## 👥 Team (ΝNTUA)
* **[Αργύριος Εξαρχάκος](https://github.com/Argyexar)**
* **[Γεωργία Παναγοπούλου](https://github.com/georgiaapn)**
