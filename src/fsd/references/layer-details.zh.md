# FSD 各层详解

Feature-Sliced Design 架构中六个层次的详细说明。

## shared（共享层）— 技术基础设施

**定位**：应用的技术基础，与业务无关。

```
shared/
├── api/          # API 客户端配置（axios 实例、拦截器等）
├── ui/           # UI 组件库（按钮、输入框、弹窗等）
├── lib/          # 工具库（日期处理、字符串格式化等）
├── config/       # 环境变量、全局常量
└── routes/       # 路由配置（不含业务逻辑）
```

**特点**：高度可复用，严格禁止包含业务逻辑。

## entities（实体层）— 业务概念模型

**定位**：核心业务实体，代表业务领域的概念。

```
entities/
├── user/         # 用户实体
│   ├── ui/       # 用户头像、信息展示组件
│   ├── model/    # 用户数据类型、验证规则
│   └── api/      # 用户相关 API 调用
├── product/      # 商品实体
└── order/        # 订单实体
```

**特点**：跨功能复用，代表业务"名词"。数据模型是核心，api 和 ui 围绕模型展开。

## features（特性层）— 用户价值功能

**定位**：用户关心的具体交互功能。

```
features/
├── user-auth/    # 用户认证
│   ├── ui/       # 登录/注册表单
│   ├── model/    # 认证状态管理
│   └── api/      # 登录/注册 API 调用
├── add-to-cart/  # 加入购物车
└── search/       # 搜索功能
```

**关键原则**：不是所有东西都是 feature，要有复用价值。单页面一步操作可合并到 entity。

## widgets（部件层）— 大型 UI 区块

**定位**：自包含的大型 UI 块，组合多个 features/entities。

```
widgets/
├── header/           # 顶部导航
├── product-grid/     # 商品网格
├── shopping-cart/    # 购物车侧边栏
└── user-profile/     # 用户资料卡
```

**使用场景**：
- 跨页面复用的大型 UI 区块
- 页面中独立的复杂区块（需要组合多个 feature）
- 嵌套路由中的完整路由块

## pages（页面层）— 完整页面

**定位**：用户访问的完整页面/屏幕。

```
pages/
├── home/            # 首页
├── product/         # 商品详情页
├── cart/            # 购物车页面
└── profile/         # 个人资料页
```

**特点**：可以包含不复用的 UI 组件。主要负责组合 widgets，不包含复杂业务逻辑。

## app（应用层）— 应用全局配置

**定位**：应用运行所需的基础设施。

```
app/
├── routes/          # 路由配置
├── store/           # 全局状态配置
├── styles/          # 全局样式
└── providers/       # 上下文提供者（Redux Provider、Theme Provider 等）
```
