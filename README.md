# china-weather-query

China weather query tool based on the National Meteorological Center (NMC) public APIs.

## Features

- Query by city name, with optional province for disambiguation.
- Query directly by station code.
- Get real-time weather and multi-day forecast.
- Output as readable text or JSON.
- Local station cache for faster repeated lookups.

## Project Structure

- `scripts/query_weather.py`: main CLI query script
- `references/api.md`: API and field notes
- `SKILL.md`: Codex skill instructions
- `agents/openai.yaml`: skill UI metadata

## Requirements

- Python 3.9+
- Network access to `https://www.nmc.cn`

No third-party Python dependencies are required.

## Quick Start

```bash
python scripts/query_weather.py --city Nanjing
python scripts/query_weather.py --city Nanjing --days 7
python scripts/query_weather.py --city Chaoyang --province Beijing --format json
python scripts/query_weather.py --station Wqsps --days 5 --format json
```

## CLI Options

```text
--city      City or district name
--province  Province name for disambiguation
--station   Station code (skip city matching)
--days      Forecast days (default: 3)
--format    text | json (default: text)
```

## Data Source

- Base: `https://www.nmc.cn/rest`
- Endpoints:
  - `/province`
  - `/province/{province_code}`
  - `/weather?stationid={station_code}`

## Notes

- Weather data depends on NMC publishing updates.
- Some city names are duplicated across provinces. Use `--province` if needed.
- First full lookup may create `scripts/.station_cache.json`.

