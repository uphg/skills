# 修改记录

## 2026-09-02 — 精简 Preferences;新增 evals.json

- 删除 `Preferences` 一节:"优先 TypeScript"、"优先 defineComponent + .tsx"、"始终 Composition API" 与开头引言重复;两条独有规则(深响应式非必需时优先 `shallowRef`、避免响应式 Props 解构)并入引言。与此前 js-coding-style 的 Preferences 清理同标准。
- 新增 evals.json:3 条测试提示词(SFC → TSX 改写、防抖搜索与侦听器清理、shallowRef 性能取舍),含 `expectations`。
- frontmatter `metadata.version` → `2026.9.2`。

## 2026-08-25 — description 措辞优化

参照示例技能库的最佳实践重写 front-matter description：动词开头、具体触发场景、明确的负面边界，并交叉引用姊妹技能。技能内容无改动。

### 变更清单

### 1. `SKILL.zh.md` → 修订 description

- 开头改为动词引导：“使用 defineComponent() + .tsx 配合 Composition API 开发 Vue 3 组件”，替代原来的名词罗列。
- 补充覆盖锚点：响应式基础（ref、shallowRef、computed）与泛型组件。
- 新增触发场景：将 SFC（`<script setup>`）改写为 defineComponent/TSX。
- 新增负面边界：不适用于组件库书写规范（使用 vue-component-authoring）或样式/主题决策。

### 2. `SKILL.md` → 同步

英文版同步以上修改，结构与中文版一致。

将 Vue3 文档的偏好从 `.vue` `<script setup lang="ts">` 语法优先改为 `defineComponent()` + `.tsx` 写法。

## 变更清单

### 1. `SKILL.md`

| 位置 | 修改前 | 修改后 |
|------|--------|--------|
| 标语 (L12) | `Always use Composition API with <script setup lang="ts">` | `Always use Composition API with defineComponent() + .tsx` |
| 偏好 (L17) | `Prefer <script setup lang="ts"> over <script>` | `Prefer defineComponent() + .tsx over <script setup lang="ts"> + .vue` |
| 引用表 (L26) | `Script Setup & Macros` → `script-setup-macros.md` | `defineComponent + TSX` → `define-component-tsx.md` |
| Component Template (L37-62) | `.vue` SFC with `<script setup lang="ts">` | `.tsx` with `defineComponent()` function signature |
| description (frontmatter) | `script setup macros, defineProps/defineEmits/defineModel` | `defineComponent + TSX` |

### 2. `references/script-setup-macros.md` → 删除

已移除。SFC 宏（`defineProps`、`defineEmits`、`defineModel`、`defineOptions`、`defineSlots`）不适用于 `.tsx` 写法。

### 3. `references/define-component-tsx.md` → 新建

覆盖以下主题：
- `defineComponent()` Options Signature
- `defineComponent()` Function Signature (3.3+)
- Props 声明（runtime declaration + `PropType`）
- Emits 声明
- Generics 泛型组件
- Expose
- `defineAsyncComponent`
- Custom Directives in TSX
- Webpack Treeshaking 注意事项

### 4. `references/advanced-patterns.md`

| 位置 | 修改说明 |
|------|----------|
| Transition | 模板语法 → TSX/JSX，`v-if` 改为 `{condition && element}` |
| TransitionGroup | `v-for` 改为 `.map()` |
| Teleport | 模板语法 → TSX/JSX |
| Suspense | 模板语法 → TSX/JSX，命名 slots 使用对象语法 `{{ default: () => ..., fallback: () => ... }}` |
| KeepAlive | 模板语法 → TSX/JSX，动态组件用 `h(component)` |
| v-memo / v-once | 标注为 template-only，无 TSX 等价写法 |
| Custom Directives | `<script setup lang="ts">` → `defineComponent` setup 函数内定义 |

### 5. `references/core-new-apis.md`

**无改动。** 该文件仅涉及 Composition API（reactivity、watchers、lifecycle、composables），不包含 SFC 特定语法，适用于 `.tsx` 和 `.vue` 两种写法。
