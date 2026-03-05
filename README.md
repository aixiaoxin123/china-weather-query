# china-weather-query

一个基于中国气象局国家气象中心（NMC）公开接口的天气查询工具。  
支持按城市/省份或站点编码查询中国城市的实时天气与未来多日预报。

## Features

- 按城市查询天气（如 `南京`、`北京`）
- 按省份消歧（如 `朝阳` + `北京市`）
- 按站点编码直接查询（如 `Wqsps`）
- 返回文本格式或 JSON 结构化结果
- 内置站点缓存，减少重复查询耗时

## Project Structure

- `scripts/query_weather.py`: 主查询脚本
- `references/api.md`: NMC 接口说明与字段说明
- `SKILL.md`: Codex Skill 说明
- `agents/openai.yaml`: Skill UI 元数据

## Requirements

- Python 3.9+
- 网络可访问 `https://www.nmc.cn`

不依赖第三方 Python 包（仅使用标准库）。

## Quick Start

```bash
python scripts/query_weather.py --city 南京
python scripts/query_weather.py --city 南京 --days 7
python scripts/query_weather.py --city 朝阳 --province 北京市 --format json
python scripts/query_weather.py --station Wqsps --days 5 --format json
```

## CLI Options

```text
--city      城市或区县名称
--province  省份名称（用于重名城市消歧）
--station   站点编码（提供后将跳过城市匹配）
--days      预报天数，默认 3
--format    输出格式：text 或 json，默认 text
```

## Data Source

- NMC API base: `https://www.nmc.cn/rest`
- 典型接口：
  - `/province`
  - `/province/{province_code}`
  - `/weather?stationid={station_code}`

## Notes

- 天气数据以 NMC 实时发布为准。
- 部分城市可能重名，建议配合 `--province` 提高匹配准确度。
- 首次查询会拉取站点列表并缓存到 `scripts/.station_cache.json`。

