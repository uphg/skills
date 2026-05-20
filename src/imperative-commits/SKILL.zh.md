---
name: imperative-commits
description: 强制执行 Git 提交信息的祈使句规范：首字母大写、动词原形、不以句号结尾。当用户询问提交信息规范或验证提交质量时使用。
---

# 祈使句提交规范

让 Git 提交标题读起来像一条命令：用 `添加功能` 而不是 `添加了功能` 或 `添加着功能`。

## 四条规则

1. **祈使语气** — 读起来像命令
2. **首字母大写** — 第一个字母大写（英文）
3. **动词原形** — 使用 `Add`、`Fix`、`Update`（不用 `Added`、`Fixes`、`Updating`）
4. **不以句号结尾**

## 检验方法

把你的标题填入这句话，看是否通顺：*“如果应用这个提交，它将 ______”*

## 正确 vs 错误

| ✅ 正确 | ❌ 错误 |
|---------|----------|
| `Add login validation` | `added login validation` |
| `Fix memory leak` | `Fixes memory leak.` |
| `Update README` | `Updating README` |

## 与约定式提交的区别

约定式提交与祈使句提交是两种不同规范，只能二选一：`feat: add login`（约定式提交）或 `Add login`（祈使句），不可混用

## 快速检查清单

- 首字母大写？✅
- 动词原形？✅
- 不以句号结尾？✅
- 通过“如果应用...”检验？✅