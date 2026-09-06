# 修改记录

## 2026-09-06 — 重写数据转换命名:仅保留 `to` / `from` / `as`

数据转换命名模块改为三条按返回值类型变化区分的方向性规则,删除 `parse`、`convert` 两套转换命名模式。

### 变更清单

### 1. `SKILL.md` → 规则替换

- 类型转换为三行,按返回值类型语义区分:
  - `to` + 目标——变成目标类型/实体(返回值类型改变):`toNumber()`、`toInt()`、`toUserVO()`。
  - `from` + 源——从源格式生成数据(反解):`fromUnixTime()`、`fromBase64()`。
  - `as` + 格式——改变展示格式(返回值类型不变):`asCamelCase()`、`asPercent()`。
- 删除 `parse` + 类型、`convert` + 模式两行——数据转换命名只用 `to` / `from` / `as`。
- 新增消歧规则:以返回值类型变化为判别依据,数据转换命名禁用 `parse`/`convert`。
- frontmatter description 锚点更新:数据转换 `to/parse/convert` → `to/from/as`;`metadata.version` → `2026.9.3`。

### 2. `SKILL.zh.md` → 同步,结构一致

### 3. `evals.json` → eval 2 更新

字符串转数字的期望改为 `to`/`from` 命名(如 `toNumber` / `fromPercent`),不再接受 `to`/`parse`/`convert` 模式。

## 2026-09-02 — 新增 evals.json

按 skill-dev Step 5 与 skill-creator schema 新增 3 条测试提示词(含 expected_output 与 expectations)。覆盖:声明方式与调用顺序、命名审查、api 与 Async 消歧。SKILL.zh.md 内容无改动。

## 2026-09-02 — 重构章节布局:代码结构与命名分离

围绕文档的两个本质问题——代码如何组织、事物如何命名——重新组织章节布局。此前评审发现存在归类错位与内容重叠。所有规则、示例与注意事项均保留,本次仅调整信息架构。

### 变更清单

### 1. `SKILL.md` → 章节重构

- 将两个近似重复的顶级标题 `Function Declaration Style` 与 `Function Declaration Order` 合并为 `Function Structure`,下设 `Declaration` 与 `Call Order` 两个子节。
- 新增统一的 `Naming Conventions` 伞层:
  - 将四个函数命名子节(事件处理器、数据转换、异步函数、存储与缓存访问)收拢为一张速查表(`场景 | 模式 | 示例`),表下保留三条消歧规则(`Async` 后缀语义;`api` + 动词永不与 `Async` 混用,不用 `asyncFetch`/`doRequest`;`read`/`write` 与 `get`/`set` 按数据位置选择)。
  - 将 `Constants` 移出"函数命名约定"——常量不是函数——与 `Variable Naming` 合并为 `Variables & Constants`。
  - 顶级章节 `File Naming` 与 `Names to Avoid` 归入 `Naming Conventions`。
- 顶级章节从 9 个收敛到 4 个:何时使用此技能 → 函数结构 → 命名约定 → 核心原则。
- `to` / `parse` / `convert` 保留为三行独立表格行,维持类型转换/字符串解析/复杂转换的区分。
- frontmatter `metadata.version` → `2026.9.2`。

### 2. `SKILL.zh.md` → 同步

中文版同步重构,与英文版结构完全一致。

## 2026-09-01 — 重命名为 `js-coding-style` 并扩充覆盖面

为避免与 skills 生态中的同名技能冲突,将技能从 `javascript` 重命名为 `js-coding-style`,并补齐评审发现的覆盖缺口:异步函数命名、常量、文件命名。删除冗余的 Preferences 一节,并说明入口函数置顶约定的意图。

### 变更清单

### 1. 目录与名称 → `js-coding-style`

- `skills-zh/javascript/` → `skills-zh/js-coding-style/`;`skills/javascript/` → `skills/js-coding-style/`。
- frontmatter `name` 更新为 `js-coding-style`;README 安装命令同步更新。

### 2. `SKILL.zh.md` / `SKILL.md` → 内容更新(同步)

- 删除 `Preferences` 一节——其三条内容与后文各节完全重复。
- 新增说明:入口函数置顶是本代码库刻意的阅读顺序约定,而非通用最佳实践。
- “命名约定”更名为“函数命名约定”;变量命名、文件命名提升为独立顶级章节。
- 重写异步函数命名:返回 Promise 的函数统一加 `Async` 后缀(`initSettingsAsync`);网络/API 请求用 `api` 前缀 + 动词(`apiGetUser`、`apiDeleteOrder`),不加 `Async` 后缀——两者不混用。
- 新增一节:存储与缓存访问动词——持久化存储用 `read`/`write`(`readSettingsFromStorage`、`writeSettingsToStorage`),内存缓存与应用状态用 `get`/`set`(`getCachedUser`、`setCachedUser`)。
- 新增一节:常量(模块级用 `UPPER_SNAKE_CASE`,函数作用域用小驼峰)。
- 新增一节:文件命名(`kebab-case`)。
- description 补充新的命名锚点(Async 后缀、api+动词、read/write、get/set、常量、文件)。
- “应避免的命名”中 `handle()` 一条交叉引用 `on[Event]` 规则。

## 2026-08-25 — description 措辞优化

重写 front-matter description：开头改为直接陈述规则的动词句，保留具体命名锚点，新增引号触发语示例与明确的负面边界。技能内容无改动。

### 变更清单

### 1. `SKILL.zh.md` → 修订 description

- 开头改为祈使式（“套用 JavaScript 编码规范：…”），替代原来的主题罗列名词短语。
- “使用时机”扩展：明确代码审查与变量命名提问场景，并给出示例“这个处理函数该叫什么”。
- 新增负面边界：不适用于 TypeScript 类型层设计或特定框架的组件规范。

### 2. `SKILL.md` → 同步

英文版同步以上修改，结构与中文版一致。

初始创建，基于 `src/javascript/AGENT.md` 生成 JavaScript 编码规范 skill。

## 变更清单

### 1. `SKILL.md` → 新建

从 `src/javascript/AGENT.md` 总结为 SKILL 格式，包含：
- 函数声明方式：始终使用 `function` 而非箭头函数
- 函数声明与调用顺序：入口函数在顶部，定义按调用顺序排列
- 事件处理函数命名：`on[Event]` 模式
- 数据转换函数命名：`to` / `as` / `parse` / `convert` 前缀约定
- 应避免的命名：`change`、`process`、`handle`、`doConvert`
