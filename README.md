# 一页纸 · 日程规划器

> 一款零依赖、纯前端、打开即用的「一日一页纸」日程规划器。支持 4×4 网格笔记（多页）、农历生日、天气定位、四视图切换、Markdown 写作、本地数据持久化、4×4 重要性分区排序。

![页面大小](https://img.shields.io/badge/size-100KB-blue) ![依赖](https://img.shields.io/badge/dependencies-zero-green) ![离线](https://img.shields.io/badge/offline-friendly-brightgreen)

---

## 📑 目录

- [快速开始](#-快速开始)
- [核心功能](#-核心功能)
- [四视图详解](#-四视图详解)
- [4×4 功能分区与重要性排序](#-44-功能分区与重要性排序)
- [多页日视图](#-多页日视图)
- [重要日子（生日/纪念日）](#-重要日子生日纪念日)
- [天气与位置](#-天气与位置)
- [农历支持](#-农历支持)
- [数据存储与备份](#-数据存储与备份)
- [键盘快捷键](#-键盘快捷键)
- [视觉设计](#-视觉设计)
- [技术架构](#-技术架构)
- [常见问题](#-常见问题)
- [自定义与扩展](#-自定义与扩展)
- [更新日志](#-更新日志)

---

## 🚀 快速开始

### 直接打开

把 `one-page-planner.html` 拖到任何现代浏览器（Chrome、Safari、Firefox、Edge）就能用。

> **不需要** Node.js / npm / 任何构建工具。**不需要** 网络服务器。**不需要** 任何账号。

### 推荐启动方式（启用浏览器定位）

部分功能（天气、定位）需要 HTTPS 或 localhost 才能使用浏览器 API：

```bash
# Python 3
python3 -m http.server 8000
# 然后浏览器打开 http://localhost:8000/one-page-planner.html
```

```bash
# Node.js
npx serve .
```

### 文件清单

```
outputs/
├── one-page-planner.html    ← 主应用（100KB，单文件，零外部依赖）
└── README.md                ← 本文档
```

---

## ✨ 核心功能

| 功能              | 说明                         |
| --------------- | -------------------------- |
| **4×4 日视图**     | 默认视图，一天 16 个格子写想法（按列分区，工作/灵活/个人） |
| **多页日视图**     | 当日 16 格写满后，按提示新增下一页             |
| **周/月/年视图**     | 完整时间维度切换，跨页聚合 + 按重要性排序展示       |
| **Markdown 写作** | 每个格子支持标题、列表、加粗、链接、引用       |
| **重要日子侧边栏**     | 生日/纪念日管理，含删除               |
| **农历生日**        | 阴历每年自动转阳历显示（如：八月廿八）        |
| **天气/位置**       | 浏览器定位 → 城市名（OSM Nominatim） |
| **数据本地存储**      | localStorage 持久化，刷新不丢      |
| **响应式设计**       | 桌面、平板、手机自适应                |
| **中文界面**        | 全中文，农历日期用天干地支              |

---

## 🎨 四视图详解

### 1. 日视图（默认）

- **4×4 网格**：默认 16 个格子等分屏幕
- **左上角信息栏**：
  - 日期行：`2026 年 8 月 27 日 · 周三`
  - 重要日子列表（如有）
  - 天气+位置行（仅今天显示）：`晴 · 北京朝阳`
- **格子操作**：
  - 点击格子 → 进入编辑模式（textarea）
  - `Esc` / `⌘+Enter` → 保存
  - `Tab` → 保存并跳到下一格
  - 鼠标悬停 → 右下角显示 ✓ 完成 / ✕ 取消
  - 点击 ✓ → 画红色斜线（已完成）
  - 点击 ✕ → 画红色叉（已取消）
  - 点击 ↻ → 恢复原状
- **多页**：右下角 `◀ 第 N 页 / 共 M 页 ▶`，单页时自动隐藏。详见 [多页日视图](#-多页日视图)。

### 2. 周视图

- 7 列布局（周一到周日）
- 每列显示当天的格子内容摘要
- 完成/取消状态用颜色标识

### 3. 月视图

- 标准日历网格（6 行 × 7 列）
- 每格显示：
  - 日期号
  - 进度条（当日完成率）
  - 最多 3 条内容预览
  - 重要日子标记
- 点击日期 → 跳转到日视图

### 4. 年视图

- 12 个迷你月历（4 列 × 3 行）
- 颜色规则：
  - 浅蓝 = 有内容
  - 红色 = 有完成项
  - 金边 = 有重要日子
  - 深色 = 今天
- 点击月份 → 跳转到月视图

### 视图切换

顶部 tab 切换：`日 | 周 | 月 | 年`

---

## 🎯 4×4 功能分区与重要性排序

每个日视图的 4×4 = 16 格按下列布局分组，跨日/周/月/年视图统一按以下规则排序：

| 位置（index） | 列 (col) | 归属栏 | 用途 | 重要性 |
|---|---|---|---|---|
| 0 | 0 | 信息格 | 日期/星期/天气/重要日子 | — |
| 1, 5, 9, 13 | 1 | **工作** | 工作事项 | 同列内 row 0 > row 3 |
| 4, 8, 12 | 0 | **工作**（左栏扩展） | 工作事项 | 同列内 row 1 > row 3 |
| 2, 6, 10, 14 | 2 | **灵活** | 灵活事项 | 同列内 row 0 > row 3 |
| 3, 7, 11, 15 | 3 | **个人/人际** | 个人成长、人际关系 | 同列内 row 0 > row 3 |

> **Grid 布局示意**（CSS Grid 4 列 × 4 行，index 0 = 左上 info）：
> ```
> | 0(info) | 1(work) | 2(flex) | 3(personal) |  ← row 0
> | 4(work) | 5(work) | 6(flex) | 7(personal) |   ← row 1
> | 8(work) | 9(work) |10(flex) |11(personal) |   ← row 2
> |12(work) |13(work) |14(flex) |15(personal) |   ← row 3
> ```

### 重要性排序规则

1. **列优先级**：工作 > 灵活 > 个人
2. **同列内**：上 (row 0) > 下 (row 3)

### 实现（单一事实源）

代码里以模块级常量定义，**所有视图统一调用 `Store.getDayItemsSorted(dateStr)`**：

- `COLUMN_OF(index)` → 列号（`index % 4`）
- `ROW_OF(index)` → 行号（`Math.floor(index / 4)`）
- `COLUMN_TYPE_OF(index)` → `'info' | 'work' | 'flex' | 'personal'`
- `itemImportance(index)` → 综合分数（`colWeight * 10 + rowWeight`）
- `Store.getDayItemsSorted(dateStr)` → 返回按重要性降序的 `[{index, content, status, pageIdx}]`，**跨所有页面聚合**

月视图的 previews、周视图的 cellSummaries 都直接消费 `getDayItemsSorted` 的结果，避免各视图自行实现导致不一致。

---

## 📄 多页日视图

当某日的内容格写满（15 个内容格全部非空），在第 15 格按 `Tab` 时会弹出确认：

> **格子已用完，是否新增另一页？**
- **确定**：创建新页，自动跳转到新页第 1 格并进入编辑
- **取消**：停留在原格

### 数据结构

```js
// 旧 (v1): 单页
cells[dateStr] = { 0: {...}, 1: {...}, ..., 15: {...} }

// 新 (v2+): 多页
cells[dateStr] = { pages: [ {...page0...}, {...page1...}, ... ] }
```

info 格（index 0）每页共享，不复制。

### 自动迁移

启动时 `Store.load()` 会检测旧格式（`day` 是普通对象，无 `.pages` 属性）并自动包一层 `{ pages: [day] }`。**用户无感知**。

### 导航

日视图右下角：`◀ 第 N 页 / 共 M 页 ▶`，单页时自动隐藏。`<◀` / `<▶>` 切换前后页（边界禁用）。

### API

- `Store.getPageCount(dateStr)` → 页数
- `Store.ensurePage(dateStr, pageIdx)` → 创建/确保某页存在
- `Store.isPageFilled(dateStr, pageIdx)` → 该页 15 格是否全部非空
- `Store.getCell(dateStr, index, pageIdx)` → 读单格（默认当前页）

---

## ⭐ 重要日子（生日/纪念日）

### 侧边栏管理

页面右侧 220px 宽的侧边栏，列出所有重要日子，每条带 🗑 删除按钮。

- 点击 `+` 打开添加对话框
- 点击 `‹` 收起侧边栏（节省屏幕空间）

### 阳历添加

```
[阳历] [🌙 阴历]              ← 切换日历类型
选择日期
[📅 2026-08-28]              ← 全宽日期选择
[✓] 每年重复                  ← 勾选后每年都显示
标题
[生日________________]        ← 全宽标题输入
                    [添加]
```

### 阴历添加

```
[阳历] [🌙 阴历]
选择月日
[正月▼]  [初一▼]            ← 月日下拉
[☐ 闰月（少数情况）]          ← 闰月选项
[✓] 每年重复                  ← 默认勾选
（取消勾选后显示 ↓）
指定年份（一次性）
[2026]                       ← 年份输入
标题
[农历生日________________]
                    [添加]
```

### 重要日子类型

| 类型       | 存储                                                   | 每年显示      |
| -------- | ---------------------------------------------------- | --------- |
| 阳历每年重复   | `{type:'solar', month, day, recurring:true}`         | ✓ 每年同月同日  |
| 阳历一次性    | `{type:'solar', date, recurring:false}`              | 仅指定日期     |
| 阴历（默认每年） | `{type:'lunar', month, day, isLeap, recurring:true}` | ✓ 每年自动转阳历 |
| 阴历一次性    | 转阳历后存 `{type:'solar', date, recurring:false}`        | 仅指定那一年    |

### 显示效果

- 信息栏：金色左边框 + ⭐ 标记
- 侧边栏：完整日期 + 标签
- 月视图：日期格右下角 ⭐ 标记 + 标题
- 年视图：日期格金色边框

### 数据迁移

旧版本（带完整日期的阳历）首次打开会自动转为「每年重复」模式。

---

## 🌤️ 天气与位置

### 数据源优先级

1. **用户手动设置**（最高优先级，永远用用户输入的城市）
2. **浏览器定位**（navigator.geolocation）+ OSM Nominatim 反向地理编码（中文结果）
3. **wttr.in** 天气 API（兜底）

### 中国大陆优化

- 天气 API：`wttr.in/北京?lang=zh` 强制中文
- 反向地理编码：使用 `accept-language=zh-CN` 返回中文城市名
- 浏览器定位：精确到区县级（如"北京朝阳"）

### 使用方法

1. **首次使用**：点信息栏的 📍 按钮 → 浏览器弹权限请求 → 同意
2. **手动改城市**：点文字本身 → 输入城市名
3. **强制刷新**：点 ↻ 按钮

### 显示规则

- **仅今天**显示天气/位置（避免历史/未来日期显示假数据）
- 提取纯描述（去温度）：`晴 23°C` → `晴`
- 仅显示省市级：`北京朝阳`、`山西运城`

### 离线/拒绝定位

如果拒绝定位或网络失败：

- 信息栏显示 `点击 📍 获取位置`
- 用户可手动输入城市
- 天气失败不影响位置显示

### 完全禁用

如果完全不需要天气/位置功能，可在浏览器控制台执行：

```js
Weather.disable()
```

---

## 🌙 农历支持

### 数据范围

- 1900 年 - 2099 年（200 年）
- 自动处理闰月
- 修复了时区 bug（用 `Date.UTC` 避免 1900 年前历史时区差异）

### 农历日期显示

- **正月**、**二月**、**三月**、...、**腊月**（不用数字 1-12）
- **初一**、**初二**、...、**廿八**、**三十**（不用 1-30）
- 闰月：🌙 闰八月

### 干支纪年（可选）

`formatLunar` 和 `lunarYearGanZhi` 提供天干地支：

- 2026 年是 **丙午年（马年）**

### API 使用

```js
// 阴历转阳历
const solar = lunarToSolar(2026, 8, 28, false);  // 2026 农历八月廿八

// 阳历转阴历
const lunar = solarToLunar(new Date('2026-10-08'));  // → {year:2026, month:8, day:28, isLeap:false}

// 格式化
formatLunar(8, 28, false);  // "八月廿八"
```

### 已知限制

由于数据表来源限制，个别日期可能差 1 天。春节、农历新年等主要节日准确。

---

## 💾 数据存储与备份

### 存储位置

所有数据存在浏览器的 `localStorage` 中，key 为 `one-page-planner:v1`。

### 数据结构

```js
{
  cells: {
    "2026-08-27": {
      0: { content: "...", status: "active|completed|cancelled" },
      1: { content: "...", status: "..." }
    }
  },
  importantDays: [
    { id, type: 'solar', date, month, day, recurring, title },
    { id, type: 'lunar', month, day, isLeap, title }
  ],
  settings: {
    location, weather, city, locationSource, geo, weatherDisabled
  }
}
```

### 备份与迁移

**导出**（浏览器控制台）：

```js
copy(localStorage.getItem('one-page-planner:v1'))
```

**导入**：

```js
localStorage.setItem('one-page-planner:v1', '/* 粘贴的 JSON */');
location.reload();
```

**清空**：

```js
localStorage.clear();
location.reload();
```

### 数据迁移

应用自动处理：

- 旧版阳历带完整日期 → 转为 `recurring: true`（每年重复）
- 缺失字段自动补全（如 `weatherDisabled`、`locationSource`）

---

## ⌨️ 键盘快捷键

| 按键                       | 动作                                   |
| ------------------------ | ------------------------------------ |
| `←` / `→`                | 上一个/下一个（按当前视图单位：日=1天，周=7天，月=1月，年=1年） |
| `T`                      | 跳到今天                                 |
| `1` / `2` / `3` / `4`    | 切到日/周/月/年视图                          |
| `Esc`                    | 退出格子编辑并保存                            |
| `⌘+Enter` / `Ctrl+Enter` | 退出格子编辑并保存                            |
| `Tab`                    | 在格子间跳转编辑                             |
| `Enter`                  | 在重要日子标题框按 Enter = 添加                 |

---

## 🎨 视觉设计

### 设计原则

- **极简主义**：米白底 + 衬线字体 + 淡彩阴影
- **低饱和度**：米白、浅灰、淡蓝为主
- **艺术感**：适当留白、对称布局
- **功能优先**：所有元素都服务于信息呈现

### 配色

| 元素   | 颜色                                                |
| ---- | ------------------------------------------------- |
| 主背景  | `#FBF8F3`（暖米白）                                    |
| 卡片背景 | `#FFFFFF`                                         |
| 信息栏  | `#F5F1EA`（暖灰）                                     |
| 文本主色 | `#2C2A26`                                         |
| 强调红  | `#E74C3C`（完成/取消）                                  |
| 重要金  | `#C9A55A`                                         |
| 字体   | `Georgia`, `Source Han Serif SC`, `Noto Serif SC` |

### 字号

- 信息栏日期：13px
- 格子正文：13px
- 月份标题：14px
- 年份：11px（紧凑）

---

## 🏗️ 技术架构

### 文件结构（单文件）

```
one-page-planner.html (~100KB)
├── <style>          ← 所有 CSS (~35KB)
│   ├── 变量定义
│   ├── 基础布局
│   ├── 4×4 网格
│   ├── 信息栏
│   ├── 重要日子侧边栏
│   ├── 模态框
│   ├── 4 个视图
│   └── 响应式断点
└── <script>        ← 所有 JS (~55KB)
    ├── Store           ← localStorage 封装
    ├── DateUtil        ← 日期工具
    ├── Markdown 解析器
    ├── Weather         ← 天气/位置模块
    ├── Lunar Calendar  ← 农历系统
    ├── DayView         ← 日视图
    ├── WeekView        ← 周视图
    ├── MonthView       ← 月视图
    ├── YearView        ← 年视图
    └── App             ← 控制器
```

### 核心模块

**Store**（数据层）：

```js
Store.getCell(dateStr, index)
Store.setCell(dateStr, index, content)
Store.setCellStatus(dateStr, index, status)
Store.getImportantDaysForDate(dateStr)
Store.addImportantDay({ type, ... })
Store.removeImportantDay(id)
```

**DateUtil**（日期工具）：

```js
DateUtil.toKey(date)         // "2026-08-27"
DateUtil.weekdayCN(date)     // "星期三"
DateUtil.isSameDay(a, b)
DateUtil.startOfWeek(date)
DateUtil.addDays(d, n), addMonths, addYears
```

**Weather**（天气模块）：

```js
Weather.fetch()              // 自动选择最佳数据源
Weather.getDisplayWeather()  // 获取当前显示数据
Weather.refreshGeolocation() // 重新获取浏览器位置
Weather.disable() / enable()  // 开关
```

**Lunar Calendar**（农历）：

```js
lunarToSolar(year, month, day, isLeap)  // 阴历转阳历
solarToLunar(date)                       // 阳历转阴历
formatLunar(month, day, isLeap)         // 格式化
```

### 零依赖

唯一外部资源是天气和反向地理编码的 HTTPS API 调用（可选）。
所有 JavaScript 均为手写，无 jQuery、React 等任何库。

---

## ❓ 常见问题

**Q: 数据存在哪里？会同步到云端吗？**
A: 只存在你的浏览器 localStorage，不上传任何服务器。换浏览器/换设备需要重新添加或手动导入 JSON。

**Q: 移动端能用吗？**
A: 可以。响应式设计自适应手机。侧边栏在窄屏会变成顶部抽屉。

**Q: 能多人协作吗？**
A: 当前版本不支持。所有数据都在本地。如需协作，可手动导出/分享 JSON。

**Q: 农历转换准确吗？**
A: 1900-2099 年支持。春节、农历新年等主要节日准确，个别日期可能差 1 天。

**Q: 天气不准确怎么办？**
A: 1) 点 📍 重新定位 2) 点文字手动改城市 3) 完全不需要可执行 `Weather.disable()`

**Q: 怎么改默认城市？**
A: 点信息栏的地点文字 → 输入城市名 → 自动保存。

**Q: 怎么完全删除数据？**
A: 浏览器控制台：`localStorage.clear(); location.reload()`

---

## 🔧 自定义与扩展

### 修改颜色

编辑 CSS 变量（在文件最开头）：

```css
:root {
  --bg: #FBF8F3;           /* 主背景 */
  --accent: #E74C3C;       /* 强调红 */
  --gold: #C9A55A;         /* 重要金色 */
  /* ... */
}
```

### 改格子数量

默认 4×4 = 16 格子。如需 3×3：

1. 改 `#day-view .grid` 的 `grid-template-columns: repeat(4, 1fr)` 为 `repeat(3, 1fr)`
2. 改 `for (let i = 1; i < 16; i++)` 为 `i < 10`

### 加自定义天气源

替换 `Weather.fetch()` 中的 fetch URL：

```js
// 例如使用和风天气
const res = await fetch(`https://api.qweather.com/v7/weather/now?location=${city}&key=YOUR_KEY`);
```

### 加自定义视图

参考 `WeekView`、`MonthView`、`YearView` 写一个 `YearListView`：

```js
const MyView = {
  render(date) {
    const container = document.getElementById('my-view');
    // ...
  }
};
```


## 📜 更新日志

### v2.0 (2026-08-27) — 三连改

本次提交包含三个独立但相关的改动，所有改动都通过 headless Chrome + Node 自测验证。

#### 🔧 任务一：农历换算修复（P0，最高优先）

**问题**：用户报告农历八月廿八对应 2026-10-09，但权威源（寿星万年历 / 香港天文台）应是 **2026-10-08（周四）**，差 1 天。

**根因**：内置 `LUNAR_INFO` 表（1900–2099 共 200 项）从某个早期版本起就**系统性偏差 1 天**。代码的位运算逻辑（`lunarToSolar` / `solarToLunar` 等）已核验正确，bug 全部在表里。

**修复**：
- 用权威 `lunar-typescript` 库（基于香港天文台 / 寿星万年历校核）逐条比对并重新生成完整 200 项表
- 同步修正注释头（之前 "bit 15 = month 1" 与代码 `(1 << (m+3))` 矛盾）
- 在 lunarToSolar 之后插入自测 IIFE，启动时打印 `[lunar] self-tests: 10/10 通过`，后续误改 LUNAR_INFO 会立即在 console 暴露
- **零迁移负担**：老用户的 `importantDays` 只存 `{type:'lunar', month, day, isLeap, recur…}`，新表生效后自动全部正确

**验证**：1900–2099 全部 73058 个日期转换与 lunar-typescript 100% 一致。

#### 📄 任务二：格子用完 → 新增另一页

- 数据结构升级：`cells[dateStr][index]` → `cells[dateStr].pages[pageIdx][index]`
- 自动迁移：旧格式包一层 `{ pages: [oldData] }`
- 仅在第 15 格按 `Tab` 且当前页 15 格全非空时弹 `confirm`，平时绝不打扰
- info 格（index 0）每页共享，不复制
- 日视图右下角加 `◀ 第 N 页 / 共 M 页 ▶` 翻页（单页自动隐藏）
- 新增 Store API：`getPageCount` / `ensurePage` / `isPageFilled` / `getCurrentPage` / `setCurrentPage`

#### 🎯 任务三：4×4 功能分区 + 重要性排序

- 定义模块级常量 `COLUMN_OF` / `ROW_OF` / `COLUMN_TYPE_OF` / `COLUMN_WEIGHT` / `ROW_WEIGHT` / `itemImportance`
- Grid 4 列映射：`col 0`（含 info + work 扩展）`col 1` work、`col 2` flex、`col 3` personal
- 新增 `Store.getDayItemsSorted(dateStr)`：跨所有页面、按重要性降序返回 `[{index, content, status, pageIdx}]` —— 单一事实源
- `Store.getDayPreviews` 改为消费 `getDayItemsSorted` 截取 top N
- `WeekView.cellSummaries` 改为直接遍历 `getDayItemsSorted` 的结果
- 任务三自测 IIFE：24/24 通过
- `itemImportance(idx 1) > itemImportance(idx 2) > itemImportance(idx 3)`（列优先级）
- 同列内 `row 0 > row 1 > row 2 > row 3`（行优先级）
- 跨页聚合：第二页的 cell 1 正确归入 work 顶部

### 文件结构变化

```
one-page-planner/
├── one-page-planner.html  (102KB → 112KB; +1900 行, 含自测)
└── README.md              (本文件, 已加入 v2.0 章节)
```

### v1.x 历史版本

| 版本  | 日期         | 主要变化                        |
| --- | ---------- | --------------------------- |
| 1.0 | 2026-08-25 | 初版：4×4 网格 + 4 视图 + Markdown |
| 1.1 | 2026-08-25 | 农历支持 + 天气定位                 |
| 1.2 | 2026-08-25 | 重要日子侧边栏 + 农历选择器             |
| 1.3 | 2026-08-26 | UI 重设计：日期+星期单行，天气+地点单行      |
| 1.4 | 2026-08-27 | 农历每年重复、阳历/阴历一致 UX、信息栏显示重要日子 |
| 2.0 | 2026-08-27 | 农历表全量重核、4×4 多页、4×4 重要性分区       |

---

## 📄 License

MIT — 自由使用、修改、商用。

---

## 🙏 致谢

- 天气数据：[wttr.in](https://wttr.in)
- 反向地理编码：[OpenStreetMap Nominatim](https://nominatim.org/)
- 农历数据表：基于公开的中文农历算法实现
- 设计灵感：纸质计划本 + 极简数字美学


##