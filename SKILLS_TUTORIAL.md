# Skills 插件使用教程（Codex）

这份教程说明：
- 如何从 Git 拉取一个 Skill
- 拉取后应该放到哪里
- 如何触发、加载、调试 Skill

## 1. Skill 是什么

一个 Skill 本质是一个目录，最少包含 `SKILL.md`。  
Codex 通过 `SKILL.md` 里的 `name` 和 `description` 判断是否触发该技能。

典型目录：

```text
your-skill/
├─ SKILL.md
├─ agents/openai.yaml      (可选，推荐)
├─ scripts/                (可选)
├─ references/             (可选)
└─ assets/                 (可选)
```

## 2. 从 Git 拉取后放到哪里

推荐放到 Codex 的技能目录：`$HOME/.codex/skills/<skill-name>`  
Windows 常见路径：`C:\Users\<你的用户名>\.codex\skills\<skill-name>`

### 步骤 A：先从 Git 拉取

```powershell
cd D:\github_dir
git clone https://github.com/aixiaoxin123/china-weather-query.git
```

拉取后目录一般是：

`D:\github_dir\china-weather-query`

### 步骤 B：复制到 Codex skills 目录

```powershell
Copy-Item -Recurse -Force `
  "D:\github_dir\china-weather-query" `
  "$HOME\.codex\skills\china-weather-query"
```

### 步骤 C：重启 Codex 会话

重启后，Codex 会重新扫描技能目录并加载新 Skill。

## 3. 另一种放置方式（工作区模式）

如果你的环境本身已经把当前项目的 `skills/` 目录作为技能来源，  
也可以直接放在：

`<workspace>/skills/<skill-name>`

例如你当前项目中：

`D:\github_dir\skills_dir\skills\china-weather-query`

这种模式下，修改 `SKILL.md` 后同样建议重启会话。

## 4. 如何触发 Skill

有两种常见方式：

1. 显式触发：在对话中写技能名，如 `$china-weather-query`
2. 语义触发：请求内容匹配 `description`，如“查南京今天和一周天气”

## 5. 本项目的使用示例

```bash
python scripts/query_weather.py --city 南京 --days 7
python scripts/query_weather.py --city 朝阳 --province 北京市 --format json
python scripts/query_weather.py --station Wqsps --days 5 --format json
```

## 6. 常见问题

### Q1: 技能没生效

- 检查目录是否放在正确位置：
  - `$HOME/.codex/skills/<skill-name>` 或工作区 `skills/<skill-name>`
- 检查 `SKILL.md` frontmatter 是否有效（`name`、`description`）
- 重启 Codex 会话

### Q2: 拉取后不知道放哪

按第 2 节执行即可。  
核心原则：Skill 文件夹最终要位于“Codex 扫描的技能根目录”下面。

### Q3: 改了脚本但结果不变

- 确认改的是“已加载路径”里的文件，不是其他副本
- 本项目可删除缓存后重试：`scripts/.station_cache.json`

