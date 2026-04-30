# FSD E-commerce Project Example

Full directory tree of an FSD-compliant e-commerce application.

```
src/
├── app/
│   ├── routes/                 # Route configuration
│   ├── store/                  # Redux store configuration
│   └── providers/             # Redux Provider, React Query configuration
├── pages/
│   ├── home/                  # Home page
│   │   └── ui/
│   │       └── HomePage.tsx
│   ├── product/               # Product detail page
│   │   └── ui/
│   │       └── ProductPage.tsx
│   └── cart/                  # Shopping cart page
│       └── ui/
│           └── CartPage.tsx
├── widgets/
│   ├── header/                # Top navigation (composes search, user menu features)
│   │   └── ui/
│   │       └── Header.tsx
│   ├── product-grid/          # Product grid
│   │   └── ui/
│   │       └── ProductGrid.tsx
│   └── shopping-cart-sidebar/ # Shopping cart sidebar
│       └── ui/
│           └── ShoppingCartSidebar.tsx
├── features/
│   ├── user-auth/             # User authentication
│   │   ├── ui/                # Login/register forms
│   │   ├── model/             # Auth state
│   │   └── api/               # Login/register requests
│   ├── add-to-cart/           # Add to cart
│   │   ├── ui/
│   │   └── model/
│   └── search/                # Search
│       ├── ui/
│       └── model/
├── entities/
│   ├── user/                  # User entity
│   │   ├── ui/                # User avatar component
│   │   ├── model/             # User type definitions
│   │   └── api/               # User info API
│   ├── product/               # Product entity
│   │   ├── ui/                # Product card component
│   │   ├── model/             # Product type definitions
│   │   └── api/               # Product list/detail API
│   └── order/                 # Order entity
│       ├── model/             # Order type definitions
│       └── api/               # Order API
└── shared/
    ├── ui/                    # UI component library (Button, Input, Modal, etc.)
    ├── api/                   # API client (axios instance, interceptors)
    ├── lib/                   # Utility functions
    └── config/                # Environment variable configuration
```
