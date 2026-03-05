# Skills 使用说明（Codex / OpenClaw）

本文补充说明：如何从 Git 下载 Skills，并放到正确目录后使用。

## 1. Skill 基本结构

一个 Skill 是一个目录，至少包含 `SKILL.md`（带 YAML frontmatter）。

```text
your-skill/
├─ SKILL.md
├─ agents/openai.yaml      (可选)
├─ scripts/                (可选)
├─ references/             (可选)
└─ assets/                 (可选)
```

## 2. 从 Git 下载 Skill

```powershell
cd D:\github_dir
git clone https://github.com/aixiaoxin123/china-weather-query.git
```

下载后目录示例：

`D:\github_dir\china-weather-query`

## 3. OpenClaw 中如何使用

OpenClaw 使用兼容 AgentSkills 的 `skills` 文件夹来教智能体如何使用工具。  
每个 Skill 都是一个带 `SKILL.md` 的目录。

### 3.1 直接放入工作区（最简单）

把仓库目录放入工作区的 `skills` 下即可使用：

`<workspace>/skills/china-weather-query`

示例（PowerShell）：

```powershell
Copy-Item -Recurse -Force `
  "D:\github_dir\china-weather-query" `
  "D:\your_workspace\skills\china-weather-query"
```

### 3.2 OpenClaw Skills 加载位置

OpenClaw 会从以下位置加载 Skills：

1. 内置 Skills（安装包自带）
2. 托管/本地 Skills：`~/.openclaw/skills`
3. 工作区 Skills：`<workspace>/skills`

### 3.3 名称冲突优先级

如果 Skill 名称冲突，优先级为：

`<workspace>/skills`（最高）  
→ `~/.openclaw/skills`  
→ 内置 Skills（最低）

另外，你可以在 `~/.openclaw/openclaw.json` 中通过  
`skills.load.extraDirs` 配置额外 Skills 目录（最低优先级）。

示例：

```json
{
  "skills": {
    "load": {
      "extraDirs": [
        "D:/shared/openclaw-skills"
      ]
    }
  }
}
```

## 4. Codex 中如何使用（补充）

如果你在 Codex 环境，常见放置目录是：

`$HOME/.codex/skills/<skill-name>`

或当前工作区：

`<workspace>/skills/<skill-name>`

修改 Skill 后建议重启会话，让扫描结果更新。

## 5. 本项目使用示例

```bash
python scripts/query_weather.py --city 南京 --days 7
python scripts/query_weather.py --city 朝阳 --province 北京市 --format json
python scripts/query_weather.py --station Wqsps --days 5 --format json
```

## 6. 常见问题

### Q1: 放进去后没生效

- 检查目录层级是否正确（必须是 `.../skills/<skill-name>/SKILL.md`）
- 检查 `SKILL.md` frontmatter 是否有效（`name`、`description`）
- 重启 OpenClaw/Codex 会话

### Q2: 多个同名 Skill 不知道用了哪个

按优先级判断：工作区 > 用户目录 > 内置。

### Q3: 改了脚本但结果没变

- 确认改的是“实际加载目录”里的文件
- 本项目可清理缓存后重试：`scripts/.station_cache.json`

