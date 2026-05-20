# 修改记录

初始创建，基于 `src/javascript/AGENT.md` 生成 JavaScript 编码规范 skill。

## 变更清单

### 1. `SKILL.md` → 新建

从 `src/javascript/AGENT.md` 总结为 SKILL 格式，包含：
- 函数声明方式：始终使用 `function` 而非箭头函数
- 函数声明与调用顺序：入口函数在顶部，定义按调用顺序排列
- 事件处理函数命名：`on[Event]` 模式
- 数据转换函数命名：`to` / `as` / `parse` / `convert` 前缀约定
- 应避免的命名：`change`、`process`、`handle`、`doConvert`
