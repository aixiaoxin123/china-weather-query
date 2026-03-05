# china-weather-query

基于中国气象局国家气象中心（NMC）公开接口的天气查询工具。  
A weather query tool powered by National Meteorological Center (NMC) public APIs.

## 功能 Features

- 按城市查询天气，可选省份消歧。  
  Query by city name, with optional province disambiguation.
- 按站点编码直接查询。  
  Query directly by station code.
- 获取实时天气和未来多日预报。  
  Get real-time weather and multi-day forecast.
- 支持文本输出和 JSON 输出。  
  Output as readable text or JSON.
- 内置站点本地缓存，重复查询更快。  
  Local station cache for faster repeated lookups.

## 项目结构 Project Structure

- `scripts/query_weather.py`: 主查询脚本 / main CLI query script
- `references/api.md`: 接口说明 / API and field notes
- `SKILL.md`: Codex 技能说明 / Codex skill instructions
- `agents/openai.yaml`: 技能 UI 元数据 / skill UI metadata

## 环境要求 Requirements

- Python 3.9+
- 可访问 `https://www.nmc.cn` 的网络环境  
  Network access to `https://www.nmc.cn`

无需第三方 Python 依赖，仅使用标准库。  
No third-party Python dependencies are required.

## 快速开始 Quick Start

```bash
python scripts/query_weather.py --city 南京
python scripts/query_weather.py --city 南京 --days 7
python scripts/query_weather.py --city 朝阳 --province 北京市 --format json
python scripts/query_weather.py --station Wqsps --days 5 --format json
```

## 参数说明 CLI Options

```text
--city      城市或区县名 / City or district name
--province  省份名(用于重名消歧) / Province for disambiguation
--station   站点编码(跳过城市匹配) / Station code (skip city matching)
--days      预报天数(默认3) / Forecast days (default: 3)
--format    输出格式 text|json / Output format text|json (default: text)
```

## 数据来源 Data Source

- Base: `https://www.nmc.cn/rest`
- Endpoints:
  - `/province`
  - `/province/{province_code}`
  - `/weather?stationid={station_code}`

## 注意事项 Notes

- 天气数据以 NMC 发布为准。  
  Weather data depends on NMC updates.
- 部分城市存在重名，建议配合 `--province` 使用。  
  Some city names are duplicated across provinces. Use `--province` when needed.
- 首次全量查询会生成 `scripts/.station_cache.json` 缓存。  
  First full lookup may create `scripts/.station_cache.json`.

## 许可证 License

本项目使用 MIT 许可证，详见 [LICENSE](./LICENSE)。  
This project is licensed under the MIT License. See [LICENSE](./LICENSE).

## 使用手册 Manual

- Skills 使用手册（Codex / OpenClaw）: [SKILLS_TUTORIAL.md](./SKILLS_TUTORIAL.md)

## 作者 Author

- 作者 / Author: AI小新
- 微信公众号 / WeChat Official Account: AI小新
