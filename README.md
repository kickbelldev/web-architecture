# web-architecture

A fully opinionated web service architecture.

## 🏗️ Architecture Overview

This repository demonstrates a hybrid approach that leverages Next.js filesystem routing for natural domain boundaries while maintaining FSD's layer-segement concept for code organization and reusability.

## 📁 Project Structure

```
src/
├── app/
│   ├── (app)/                                 # 🔧 App layer: Global configuration/provider segments
│   │   └── <segment>/                         # react-query, config, theme, ...
│   │
│   ├── (routes)/                              # 📄 Pages + Widgets layer
│   │   ├── <slice>/                           # Route-level slice (ex: products, profile)
│   │   │   ├── {page.tsx|layout.tsx|error.tsx}...
│   │   │   ├── <segment>/                     # Segments (ex: _ui, _model, etc.)
│   │   │   │   ├── <Component>.tsx            # UI components, written in PascalCase
│   │   │   │   ├── use<Slice><What>.ts        # Hooks
│   │   │   │   └── <domain>.<suffix>.ts       # Suffixes for everything except components and hooks
│   │   │   └── <slice>/                       # Nested route slice (ex: [id], settings)
│   │   │       ├── page.tsx|layout.tsx?
│   │   │       └── <segment>/
│   │   │
│   │   └── <slice>/...                        # Other route slices
│   │
│   ├── api/                                   # 🔌 Pages + Widgets layer
│   │
│   ├─ _ui/                                    # Global UI segment
│   │   ├── <segment>.tsx
│   │   └── ...
│   ├── layout.tsx                             # Root layout
│   ├── loading.tsx                            # Global loading UI
│   ├── error.tsx                              # Global error UI
│   ├── globals.css
│   └── ...
│
├── features/                                  # ⚡ Features layer: User action-based
│   ├── <slice>/                               # auth, search, notifications
│   │   ├── <segment>/...                      # ui, model, api, lib, ...
│   │   └── index.ts                           # barrel export
│   │
│   └── <slice>/...                            # Other feature slices
│
├── entities/                                  # 🏢 Entities layer: Business entities
│   ├── <slice>/                               # user, product, order, ...
│   │   ├── <segment>/                         # ui, model, api, ...
│   │   └── index.ts                           # barrel export
│   │
│   └── <slice>/...                            # Other entity slices
│
└── shared/                                    # 🔗 Shared layer: Reusable segments
    ├── <segment>/                             # ex) ui, lib, api, types
    │   └── <element>/?                        # For UI segments: button, input, ...
    │       ├── <Component>.tsx
    │       ├── <Component>.stories.tsx?
    │       ├── <Component>.test.tsx?
    │       └── ...
    │
    └── <segment>/...
```

## 🧱 Architecture Layers

### 🔧 App Layer `(app)/`

Global application configuration and initialization.

- Uses Next.js Route Groups for URL-neutral organization
- Contains providers, global store, and app-wide settings

### 📄 Pages + Widgets Layer `(routes)/`

Route-specific domains with their associated widgets.

- Leverages Next.js filesystem routing for natural domain boundaries
- Supports hierarchical widget organization for nested routes

### ⚡ Features Layer

Business functionality modules with complete feature implementations.

### 🏢 Entities Layer

Business entity management with UI representations and data logic.

### 🔗 Shared Layer

Reusable, framework-agnostic utilities and components.

## 📊 Dependency Rules

```
app ← nested widgets ← features ← entities ← shared
```

- **Upper layers** can import from lower layers
- **Lower layers** cannot import from upper layers
- **Same layer** cross-imports are prohibited (except pages layer)

## 📝 Usage Examples

### Creating a New Page Widget

```tsx
// app/(routes)/dashboard/ui/stats-panel.tsx
import { Card } from '@/shared/ui/card'
import { useStatsData } from '../model/stats-store'

export function StatsPanel() {
  const { data, loading } = useStatsData()

  if (loading) return <div>Loading...</div>

  return (
    <Card>
      <h3>Statistics</h3>
      {/* Stats content */}
    </Card>
  )
}
```

### Using Nested Layout

```tsx
// app/(routes)/dashboard/layout.tsx
import { Sidebar, Header } from './_ui'

export default function DashboardLayout({ children }) {
  return (
    <div className="dashboard-layout">
      <Header />
      <div className="content">
        <Sidebar />
        <main>{children}</main>
      </div>
    </div>
  )
}
```

### Feature Integration

```tsx
// app/(routes)/home/page.tsx
import { LoginForm } from '@/features/auth'
import { SearchBar } from '@/features/search'
import { UserCard } from '@/entities/user'
import { HeroSection } from './_ui'

export default function HomePage() {
  return (
    <div>
      <HeroSection />
      <SearchBar />
      <LoginForm />
      <UserCard userId="123" />
    </div>
  )
}
```

## ✨ Key Benefits

- **🎯 Natural Domain Separation**: Next.js filesystem routing creates clear boundaries
- **📚 Hierarchical Organization**: Page → Section → Sub-page widget management
- **📍 Code Colocation**: Related code stays physically close
- **🔄 Scalable Structure**: Consistent patterns for new features and pages
- **⚡ Next.js Native**: Seamlessly integrates with framework conventions

## 📚 Learn More

- [Next.js Documentation](https://nextjs.org/docs)
- [Feature-Sliced Design](https://feature-sliced.design/)

## 📄 License

MIT License - see the [LICENSE](LICENSE) file for details.

---

⭐ **Star this repository if it helps you build better Next.js applications!**
