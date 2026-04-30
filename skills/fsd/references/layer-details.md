# FSD Layer Details

In-depth explanation of all six layers in the Feature-Sliced Design architecture.

## shared — Technical Infrastructure

**Purpose**: The technical foundation of the application, business-agnostic.

```
shared/
├── api/          # API client configuration (axios instance, interceptors, etc.)
├── ui/           # UI component library (buttons, inputs, modals, etc.)
├── lib/          # Utility library (date handling, string formatting, etc.)
├── config/       # Environment variables, global constants
└── routes/       # Route configuration (no business logic)
```

**Characteristics**: Highly reusable, strictly prohibits business logic.

## entities — Business Concept Models

**Purpose**: Core business entities representing business domain concepts.

```
entities/
├── user/         # User entity
│   ├── ui/       # User avatar, info display components
│   ├── model/    # User data types, validation rules
│   └── api/      # User-related API calls
├── product/      # Product entity
└── order/        # Order entity
```

**Characteristics**: Reusable across features, represents business "nouns". Data model is the core; api and ui revolve around it.

## features — User Value Functions

**Purpose**: Specific interactive functionality that users care about.

```
features/
├── user-auth/    # User authentication
│   ├── ui/       # Login/register forms
│   ├── model/    # Auth state management
│   └── api/      # Login/register API calls
├── add-to-cart/  # Add to cart
└── search/       # Search functionality
```

**Key principle**: Not everything is a feature — it must have reuse value. Single-page one-step operations can be merged into entities.

## widgets — Large UI Blocks

**Purpose**: Self-contained large UI blocks that compose multiple features/entities.

```
widgets/
├── header/           # Top navigation
├── product-grid/     # Product grid
├── shopping-cart/    # Shopping cart sidebar
└── user-profile/     # User profile card
```

**Use cases**:
- Large UI blocks reused across pages
- Independent complex blocks within a page (combining multiple features)
- Complete route blocks in nested routing

## pages — Complete Pages

**Purpose**: Full pages/screens that users access.

```
pages/
├── home/            # Home page
├── product/         # Product detail page
├── cart/            # Shopping cart page
└── profile/         # Profile page
```

**Characteristics**: May contain non-reusable UI components. Primarily composes widgets, should not contain complex business logic.

## app — Application Global Configuration

**Purpose**: Infrastructure required for the application to run.

```
app/
├── routes/          # Route configuration
├── store/           # Global state configuration
├── styles/          # Global styles
└── providers/       # Context providers (Redux Provider, Theme Provider, etc.)
```
