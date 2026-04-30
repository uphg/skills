# FSD 电商项目案例

符合 FSD 规范的电商应用完整目录树。

```
src/
├── app/
│   ├── routes/                 # 路由配置
│   ├── store/                  # Redux store 配置
│   └── providers/             # Redux Provider、React Query 配置
├── pages/
│   ├── home/                  # 首页
│   │   └── ui/
│   │       └── HomePage.tsx
│   ├── product/               # 商品详情页
│   │   └── ui/
│   │       └── ProductPage.tsx
│   └── cart/                  # 购物车页面
│       └── ui/
│           └── CartPage.tsx
├── widgets/
│   ├── header/                # 顶部导航（组合搜索、用户菜单等 feature）
│   │   └── ui/
│   │       └── Header.tsx
│   ├── product-grid/          # 商品网格
│   │   └── ui/
│   │       └── ProductGrid.tsx
│   └── shopping-cart-sidebar/ # 购物车侧边栏
│       └── ui/
│           └── ShoppingCartSidebar.tsx
├── features/
│   ├── user-auth/             # 用户认证
│   │   ├── ui/                # 登录/注册表单
│   │   ├── model/             # 认证状态
│   │   └── api/               # 登录/注册请求
│   ├── add-to-cart/           # 加入购物车
│   │   ├── ui/
│   │   └── model/
│   └── search/                # 搜索
│       ├── ui/
│       └── model/
├── entities/
│   ├── user/                  # 用户实体
│   │   ├── ui/                # 用户头像组件
│   │   ├── model/             # User 类型定义
│   │   └── api/               # 用户信息 API
│   ├── product/               # 商品实体
│   │   ├── ui/                # 商品卡片组件
│   │   ├── model/             # Product 类型定义
│   │   └── api/               # 商品列表/详情 API
│   └── order/                 # 订单实体
│       ├── model/             # Order 类型定义
│       └── api/               # 订单 API
└── shared/
    ├── ui/                    # UI 组件库（Button、Input、Modal 等）
    ├── api/                   # API 客户端（axios 实例、拦截器）
    ├── lib/                   # 工具函数
    └── config/                # 环境变量配置
```
