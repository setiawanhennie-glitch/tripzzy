# Tripzzy

Tripzzy is an AI-powered travel planner built with FastAPI, LangGraph, Groq, Tavily, AviationStack, and PostgreSQL. It generates practical trip plans using flight information, hotel research, and an AI-generated itinerary.

## Features

- Flight recommendations using AviationStack MCP tools
- Hotel research using Tavily MCP
- AI-generated itineraries and travel recommendations
- PostgreSQL-backed LangGraph conversation checkpoints
- Web interface for creating, copying, and downloading travel plans
- JSON API for integrating the planner with other applications

## Requirements

- Python 3.12 or newer
- [uv](https://docs.astral.sh/uv/) or `pip`
- A PostgreSQL database
- API keys for Groq and Tavily
- An AviationStack API key

## Installation

### Using uv

```powershell
uv sync
```

### Using pip

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Configuration

Create a `.env` file in the project root:

```dotenv
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
SUPABASE_URL=your_postgresql_connection_url
```

`SUPABASE_URL` should be the external PostgreSQL connection URL for the database used by the LangGraph checkpointer. The application adds `sslmode=require` when it is not already present.

## Running the application

Start the development server with:

```powershell
uvicorn app:app --reload
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in a browser.

The health endpoint is available at [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health).

## API usage

Send a travel request to `POST /api/travel`:

```powershell
curl -X POST http://127.0.0.1:8000/api/travel `
  -H "Content-Type: application/json" `
  -d '{\"message\":\"Plan a 7-day trip to Japan from Bangladesh with flights, hotels, and sightseeing.\"}'
```

The response includes:

- `answer`: the generated travel plan
- `thread_id`: the conversation checkpoint identifier
- `flight_results`: flight research and recommendations
- `hotel_results`: hotel research
- `itinerary`: the generated itinerary
- `llm_calls`: the number of model calls used

Pass the returned `thread_id` in a later request to continue an existing conversation.

## Project structure

```text
app.py                  FastAPI application and HTTP routes
backend.py              LangGraph travel-planning workflow
mcp_client.py           Tavily and AviationStack MCP clients
templates/index.html    Web interface markup
static/                 Frontend JavaScript and CSS
tools/                  Supporting flight and travel tools
requirements.txt        Python dependencies
```

## Workflow

1. The flight agent gathers airport and airline information.
2. The hotel agent searches for suitable hotels.
3. The itinerary agent creates a complete, budget-aware itinerary.
4. The final agent formats the travel plan for the user.

## Notes

- Flight pricing may be unavailable when the live flight provider does not return fare data.
- Keep `.env` out of source control; it is already listed in `.gitignore`.