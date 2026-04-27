# 修改记录

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
