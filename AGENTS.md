# AGENTS.md — nanobot GitHub Pages 工作台

本文件是仓库的设计与协作规范。任何对本仓库页面的新建、重构、改版，先读本文件；
冲突时按此优先级执行：用户显式指令 > 本规范 > 设计者个人偏好。

## 1. 仓库定位

- 个人静态发布工作台，托管于 GitHub Pages（master 分支直推即发布）。
- 每个页面是**独立单文件 HTML**（CSS/JS 内联，无构建步骤，离线可打开）。
- `index.html` 是「出版物目录」，新增页面必须同步登记（见 §5）。

## 2. 两套设计语言（按用途选型，不混用）

仓库并存两套视觉体系，各属其位：

### 2.1 MONO 数据报告体系（财报解读、数据分析、含量化图表的页面）

来源正本是 **lieflat-charts skill**（`/Users/ink/.pi/agent/skills/lieflat-charts/SKILL.md`），
图表必须从该 skill 的模板库生成，禁止自创「看起来差不多」的图。

- **色板**（全部来自 mono-tokens.js，无彩色）：
  - 纸 `#F0EFEB` / 墨 `#1C1C1A` / 次级 `#8F8E88` / 发丝 `#DEDDD6` / 灰阶 ladder `['#1C1C1A','#4A4944','#8F8E88','#B0AFA9','#D8D7D1']`
- **图表纪律**：
  - 选型顺序：Lupi Editorial → Lupi Basics（F1–F12）→ Glance（仅当前两组都不适配时）。
  - 每张图记录：图型编号、gallery 文件、卡内标题（如「F1 Rung Bars · basics-gallery『Revenue by plan, rung by rung』」）。
  - 保真模板骨架：rng 抖动、每 5 档点标、段间呼吸格、barcode 地板、paint-order 光晕标注、
    fade/pop/draw 动画、`obsReveal` 滚入播放 + 点击重播、`prefers-reduced-motion` 降级。
  - 硬禁令：断轴柱状图、彩色/发光/渐变、面积编码不用 sqrt、给装饰元素加交互。
- **字体**：Inter 400–800 以 data URI 内嵌（**禁 CDN 链接**），中文回退 PingFang SC；
  SVG 内中文最小字号 ≥10px（高于模板英文下限 5.5/6.5px 的中文本地化，需在附注说明）。
- **卡片**：同底色、圆角 24px、无边框无阴影，靠留白分卡。
- **表格**：发丝线、tabular-nums、重点行 `#DEDDD6` 底；涨跌标注用墨/灰明度，不引入红绿。

### 2.2 出版社目录体系（index 类导航页）

正本为 `index.html`（2026-08 版）。

- **概念**：工作台 = 「NANOBOT 自动出版社」，目录页 = 出版物目录（报头 + 馆藏章 + 目录行）。
- **色板**：纸 `#F2F1EB` / 墨 `#1B1B18` / 次级 `#6E6C64` / 发丝 `#D8D6CB` /
  邮戳红 `#B33A2B`（**仅限**印章与新刊标记，语义色不扩散）。
- **字体**：全系统字体零外链——宋体（报头/大标）× PingFang（正文）× ui-monospace（编号/日期）。
- **版式**：报头（wordmark + 大字标 + 旋转邮戳）→ 目录表头 → 发丝线目录行 →
  页脚；目录行 = №编号（沿出版时序）/ 标题 + slug + 类型标签 / 出版日期 / 悬停箭头。
- **交互**：整行可点、键盘 focus inset 描边、hover 箭头位移；行级 stagger 入场 + 印章盖落动画；
  印章动画 scale ≤1.3 且窄屏下 `align-self:center`（防瞬时横向溢出，历史 bug）。
- **单主题纸面**是刻意选择，不提供暗色切换。

## 3. 全局硬规则（两套体系共用）

- 单文件、无外部依赖（字体/库均内嵌或系统栈）；例外需用户明确许可。
- 语义 HTML；标签严格配对；body 无横向溢出——**窄屏（≤500px）与动画中途时刻都要验证**。
- `prefers-reduced-motion: reduce` 必须降级所有入场/描线动画。
- 数字统一 `font-variant-numeric: tabular-nums`。
- 动效只作解释或反馈：删掉动画后若信息无损失，就不要动画。
- 中文内容，红涨绿跌等彩色约定在 MONO 体系内不成立（用明度与文字表达方向）。
- 提交前跑 `check_secrets`。

## 4. 验证流程（当前模型无法读图时的标准替代）

1. JS 提取后 `node --check`；
2. Python HTMLParser 配对检查（无 unclosed / stray 标签）；
3. headless Chrome `--dump-dom` 注入诊断脚本：检查 SVG 渲染元素数、
   标签 bbox 越界（OVERFLOW）与两两重叠（OVERLAP）、`<title>` 混入 `<text>`、
   窄屏横向溢出（多时刻采样含动画中途）；
4. 全部通过后提交。

## 5. 目录登记与发布

- 新页面发布后同步更新 `index.html`：按「出版物目录」行结构登记
  （编号顺延、标题中文 + slug、类型标签、出版日期取 **git 真实提交时间**），新刊按发布时序置顶。
- 提交信息用中文，前缀 `feat:` / `style:` / `docs:` / `fix:`。
- 完成即 `git push origin master`（Pages 自动部署）。

## 6. 现有页面速查（各自保留既有风格，改版时先读本规范再动手）

| 页面 | 体系 | 说明 |
|---|---|---|
| index.html | 目录体系 | 出版物目录正本 |
| smic-2026q2-earnings.html | MONO 报告 | 6 张模板图（F1/L3/F2/F7/F6/F5），附注含选型记录 |
| dsh-system-flow.html | 独立 | DeepSeek Harness 流程（其自有风格） |
| agent-memory.html | 独立 | GitHub 暗色主题技术笔记 |
| 其余历史页面 | 独立 | 保持原样即可，不强行统一 |

## 7. 技能索引

- 图表：`/Users/ink/.pi/agent/skills/lieflat-charts/SKILL.md`（catalog.md 选型、templates/ 模板、mono-tokens.js 正本）
- 页面设计：`/Users/ink/.pi/agent/skills/html/SKILL.md` + `/Users/ink/.pi/agent/skills/design-artifact/SKILL.md`
