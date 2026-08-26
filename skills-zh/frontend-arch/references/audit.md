# 审查指南 —— 检查既有项目

如何机械化地检测架构违规，并按修复优先级排序。先跑 grep 再读代码——它们定位病灶；严重度需结合上下文判断。

## 目录
- [检测命令](#检测命令)
- [严重度排序](#严重度排序)

## 检测命令

```bash
# 隐藏模块接口的通配符 re-export
grep -rn "export \*" src --include="*.ts" --include="*.tsx" --include="*.js"

# 跨模块边界的深路径导入（按你的目录布局调整 pattern）
grep -rnE "from ['\"]@/(features|entities)/[^'\"]+/(model|api|ui)/" src

# 模块通过自己的 index 导入自己（循环风险）
# 在各模块文件夹里找 `from "../"` 或 `from "./index"`

# 混装多领域的大杂烩文件
find src -name "utils.ts" -o -name "helpers.ts" -o -name "types.ts" -o -name "constants.ts" | xargs wc -l | sort -rn | head

# 循环依赖（需安装 madge）
npx madge --circular --extensions ts,tsx src

# 模块内部按本质命名的文件夹（去分域化 smell）
find src -type d \( -name components -o -name hooks -o -name types \) -not -path "*/node_modules/*"
```

人工检查项：
- shared 里的单消费者代码（逐个文件问"还有谁在导入它？"）。
- feature 按 UI 位置命名（`features/header`）而非用户操作。
- token/会话状态存在某个页面或单个 feature 里。
- 无业务语境的共享 UI 组件内嵌业务逻辑。

## 严重度排序

按此顺序修复——前面的项会引发后面的项：

1. **循环依赖** —— 用耦合处理手册打破：合并模块或把共享部分下沉一层。
2. **跨页面复制的业务逻辑** —— 提升一次到更低层；分歧会让 bug 修复点成倍增加。
3. **跨模块的深路径导入** —— 改走 public API；这为其他一切的安全重构扫清障碍。
4. **shared 中的单消费者代码** —— 移回消费者旁边；给风险最高的层瘦身。
5. **大杂烩/本质命名的文件与文件夹** —— 按领域改名拆分；纯机械但涉及大量文件，按团队负责区域机会式推进。

别试图一轮修完所有问题：选一类违规、清扫干净、立刻固化为 lint 规则让它无法复发，然后处理下一类。
