# FinApp — Project Context & Documentation

> **Last updated:** 2026-02-11  
> **Status:** Production (Lovable Cloud / Supabase backend)  
> **Preview:** https://id-preview--f2e36c93-e135-4902-834f-648bc99748cf.lovable.app

---

## 1. Overview

FinApp is a full-stack personal finance management application built with **React 18 + TypeScript + Vite** on the frontend and **Supabase (Lovable Cloud)** on the backend. It allows users to manage bank accounts, track transactions, monitor investments, handle recurring bills, and generate financial reports — all in a responsive, themed UI with dark/light mode support.

---

## 2. Technology Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | React 18 + TypeScript |
| **Build Tool** | Vite |
| **Styling** | Tailwind CSS + shadcn/ui components |
| **State / Data** | TanStack React Query (server state), Zustand (client state), React Context (auth) |
| **Routing** | React Router v6 |
| **Backend** | Supabase (PostgreSQL, Auth, Edge Functions, Realtime, Storage) |
| **Forms** | React Hook Form + Zod |
| **Charts** | Recharts + Reaviz |
| **Icons** | Lucide React |
| **Animations** | Framer Motion |
| **PWA** | Custom service worker (`public/sw.js`) |

---

## 3. Project Structure

```
├── docs/
│   └── context.md              # THIS FILE — project documentation
├── public/
│   ├── manifest.json           # PWA manifest
│   ├── sw.js                   # Service worker
│   ├── pwa-*.png               # PWA icons (192, 512, maskable variants)
│   ├── robots.txt              # SEO robots file
│   └── placeholder.svg         # Default placeholder image
├── src/
│   ├── App.tsx                 # Root component — QueryClient, ThemeProvider, Router, AuthProvider
│   ├── App.css                 # Minimal app-level styles
│   ├── index.css               # Design system tokens (HSL), Tailwind layers, auth styles
│   ├── main.tsx                # Entry point — renders <App />
│   ├── components/             # All UI components (see §4)
│   ├── constants/              # App-wide constants and enums (see §5)
│   ├── contexts/               # React contexts (see §6)
│   ├── hooks/                  # Custom hooks (see §7)
│   ├── integrations/           # Supabase client config
│   ├── lib/                    # API functions and utilities
│   ├── pages/                  # Route-level page components (see §8)
│   ├── types/                  # TypeScript type definitions (see §9)
│   └── utils/                  # Pure utility functions (see §10)
├── supabase/
│   ├── config.toml             # Supabase local config
│   ├── migrations/             # Database migrations (READ-ONLY)
│   └── functions/              # Edge Functions (see §11)
├── tailwind.config.ts          # Tailwind configuration with custom tokens
├── vite.config.ts              # Vite build configuration
├── tsconfig.json               # TypeScript config (path alias: @/* → ./src/*)
└── components.json             # shadcn/ui component config
```

---

## 4. Components (`src/components/`)

### 4.1 Layout (`components/layout/`)
| File | Purpose |
|------|---------|
| `Navbar.tsx` | Top navigation bar — logo, desktop nav, user section, mobile hamburger |
| `PageLayout.tsx` | Reusable page wrapper with navbar, title, and content area |
| `MobileSidebar.tsx` | Slide-out mobile navigation drawer |
| `ThemeProvider.tsx` | Dark/light theme context provider |
| `SimpleThemeProvider.tsx` | Simplified theme provider variant |
| `PageTransition.tsx` | Route transition animations |
| `RoutePreloader.tsx` | Preloads route components for faster navigation |
| `navbar/NavbarLogo.tsx` | App logo component |
| `navbar/DesktopNavigation.tsx` | Desktop horizontal nav links |
| `navbar/DesktopUserSection.tsx` | User avatar, notifications, logout (desktop) |
| `mobile-sidebar/MobileSidebarHeader.tsx` | Mobile sidebar header with close button |
| `mobile-sidebar/MobileSidebarNavigation.tsx` | Mobile nav link list |
| `mobile-sidebar/MobileSidebarNotifications.tsx` | Mobile notification section |
| `mobile-sidebar/MobileSidebarUserSection.tsx` | Mobile user info and logout |

### 4.2 Authentication (`components/auth/`)
| File | Purpose |
|------|---------|
| `AuthLayout.tsx` | Centered auth page layout with title/subtitle |
| `AuthLayoutContainer.tsx` | Container wrapper for auth forms |
| `LoginForm.tsx` | Email/password login + Google OAuth |
| `RegisterForm.tsx` | Registration form with email/password |
| `ForgotPasswordForm.tsx` | Password reset request form |
| `ResetPasswordForm.tsx` | New password form (after reset link) |
| `VerifyEmailForm.tsx` | OTP email verification form |
| `ProtectedRoute.tsx` | Route guard — redirects unauthenticated users to `/auth` |

### 4.3 Dashboard (`components/dashboard/`)
| File | Purpose |
|------|---------|
| `BalanceSummary.tsx` | Aggregated balance card shown on home page |
| `LogoutButton.tsx` | Standalone logout button component |

### 4.4 Accounts (`components/accounts/`)
| File | Purpose |
|------|---------|
| `AccountsHeader.tsx` | Page title and description for accounts |
| `AccountsMainContent.tsx` | Main content area — account list + inline forms |
| `AccountsSidebar.tsx` | Sidebar with create/transfer actions |
| `AccountFormDrawer.tsx` | Slide-out drawer for account creation/editing |
| `InlineAccountForm.tsx` | Inline form for quick account editing |
| `InlineTransferForm.tsx` | Inline transfer form |
| `TransferDrawer.tsx` | Drawer for inter-account transfers |
| `list/AccountCard.tsx` | Individual account card display |
| `list/AccountTypeMap.ts` | Maps account types to icons/labels |
| `list/AccountsByCategory.tsx` | Groups accounts by type |
| `list/AccountsList.tsx` | Full accounts list component |
| `list/AccountsListSkeleton.tsx` | Loading skeleton for accounts |
| `list/DeleteAccountDialog.tsx` | Confirmation dialog for account deletion |
| `list/EmptyAccountsMessage.tsx` | Empty state when no accounts exist |

### 4.5 Transactions (`components/transactions/`)
| File | Purpose |
|------|---------|
| `TransactionTable.tsx` | Desktop transaction table with sorting |
| `TransactionsMobileList.tsx` | Mobile-optimized transaction list |
| `TransactionForm.tsx` | Create/edit transaction form |
| `TransferForm.tsx` | Inter-account transfer form |
| `TransactionDetail.tsx` | Transaction detail view |
| `TransactionFilters.tsx` | Filter controls (type, category, search) |
| `TransactionTypeFilter.tsx` | Income/expense/transfer toggle |
| `TransactionImport.tsx` | **Multi-step file import** — upload → preview → integration selection (AI/manual) → processing → review → import |
| `TransactionSummaryCards.tsx` | Summary metrics (totals, counts) |
| `TransactionsChart.tsx` | Visual chart of transactions over time |
| `TransactionsList.tsx` | Generic transactions list |
| `TransactionPopup.tsx` | Quick transaction popup |
| `TransactionDeleteDialog.tsx` | Delete confirmation dialog |
| `TimePeriodTabs.tsx` | Time period selector tabs |
| `form/TransactionCategorySelector.tsx` | Category dropdown for forms |
| `form/TransactionPhotoUpload.tsx` | Receipt photo upload |
| `form/TransactionTagManager.tsx` | Tag management for transactions |
| `mobile/TransactionMobileCard.tsx` | Mobile transaction card layout |

### 4.6 Bills / Recurring (`components/bills/`)
| File | Purpose |
|------|---------|
| `BillsManagement.tsx` | Main bills management component |
| `AddBillForm.tsx` | Form for adding new recurring bills |
| `AddBillSidebar.tsx` | Sidebar with add bill action |
| `BillRow.tsx` | Individual bill row in table |
| `MobileBillCard.tsx` | Mobile bill card layout |
| `RecurringTable.tsx` | Responsive recurring table wrapper |
| `RecurringTableDesktop.tsx` | Desktop recurring items table |
| `RecurringTableMobile.tsx` | Mobile recurring items list |
| `ProjectionTable.tsx` | Monthly projection forecast table |
| `YearlyNetProjection.tsx` | 12-month income vs expenses projection |
| `ExpectedEarnings.tsx` | Expected monthly income summary |
| `ExpectedPayments.tsx` | Expected monthly expense summary |
| `BillsEmptyState.tsx` | Empty state component |
| `AccountSelect.tsx` | Account selector for bill assignment |
| `SpecificDaySelect.tsx` | Day-of-month selector |
| `shared/BillFormFields.tsx` | Shared form fields for bills |
| `shared/BillFormActions.tsx` | Shared form action buttons |

### 4.7 Investments (`components/investments/`)
| File | Purpose |
|------|---------|
| `InvestmentsHeader.tsx` | Portfolio overview header with total value |
| `InvestmentAccountsList.tsx` | List of investment accounts |
| `InvestmentAccountCard.tsx` | Individual investment account card |
| `PortfolioView.tsx` | Detailed portfolio view with allocations |
| `PortfolioRebalanceDialog.tsx` | Rebalancing dialog |
| `AddAssetDialog.tsx` | Add asset to portfolio dialog |
| `EditAssetDialog.tsx` | Edit existing asset |
| `SellAssetDialog.tsx` | Sell/remove asset dialog |
| `AssetCard.tsx` | Individual asset card |
| `AssetFilters.tsx` | Filter assets by type |
| `AssetManagementDialog.tsx` | Manage portfolio assets |
| `CreateInvestmentAccountDialog.tsx` | Create new investment account |
| `PriceIndicator.tsx` | Real-time price change indicator |
| `EmptyInvestmentsMessage.tsx` | Empty state |

### 4.8 Profile / Settings (`components/profile/`)
| File | Purpose |
|------|---------|
| `ProfileForm.tsx` | Main profile settings form |
| `ProfileInfoForm.tsx` | Name/email editing |
| `ProfileImageUpload.tsx` | Avatar upload |
| `ProfilePasswordSection.tsx` | Password change section |
| `ProfileAccountDeletion.tsx` | Account deletion with confirmation |
| `CategoriesTab.tsx` | Category management tab |
| `CategoryRow.tsx` | Individual category row |
| `EditCategoryDialog.tsx` | Edit category dialog |
| `CategoryColorSelector.tsx` | Color picker for categories |
| `CategoryIconSelector.tsx` | Icon picker for categories |
| `categories/CategoryFormCard.tsx` | Category creation form |
| `categories/CategorySection.tsx` | Category section (income/expense) |
| `NotificationSettings.tsx` | Notification preferences |
| `IntegrationsTab.tsx` | API key management (OpenAI, etc.) |
| `ReportsTab.tsx` | Financial reports and analytics |
| `ReportsCharts.tsx` | Report chart visualizations |
| `ReportsMetrics.tsx` | Report metrics cards |
| `ReportsLoading.tsx` | Reports loading skeleton |
| `MetricsOverview.tsx` | Dashboard metrics overview |
| `FinancialInsights.tsx` | AI-generated financial insights |
| `ExpensesBreakdown.tsx` | Expense breakdown by category |
| `ExpensesCategoryList.tsx` | Categorized expenses list |
| `BudgetCalculator.tsx` | Budget planning calculator |
| `SavingsGoals.tsx` | Savings goals tracker |
| `ThemeSettings.tsx` | Theme customization |

### 4.9 Notifications (`components/notifications/`)
| File | Purpose |
|------|---------|
| `NotificationDropdown.tsx` | Navbar notification bell dropdown |
| `NotificationItem.tsx` | Individual notification item |

### 4.10 UI Components (`components/ui/`)
All shadcn/ui primitives (accordion, alert-dialog, avatar, badge, button, calendar, card, chart, checkbox, dialog, drawer, dropdown-menu, form, input, label, popover, progress, select, separator, sheet, skeleton, slider, switch, table, tabs, textarea, toast, tooltip, etc.) plus custom components:
- `error-boundary.tsx` — React error boundary
- `horizontal-bar-chart.tsx` — Custom horizontal bar chart
- `line-chart.tsx` — Custom line chart
- `loading-skeleton.tsx` — Page-level loading skeleton
- `loading-spinner.tsx` — Spinner component
- `theme-toggle.tsx` — Dark/light mode toggle

---

## 5. Constants (`src/constants/`)

| File | Contents |
|------|----------|
| `app.ts` | Currencies, account types, transaction types, bill repeat patterns, asset types, default colors, label mappings |
| `navigation.ts` | Navigation menu items with icons and routes |
| `categoryIcons.ts` | Available category icon list |

---

## 6. Contexts (`src/contexts/`)

| File | Purpose |
|------|---------|
| `AuthContext.tsx` | **Primary auth context** — wraps Supabase Auth. Provides `user`, `session`, `loading`, `signUp`, `signIn`, `signInWithGoogle`, `signOut`, `resetPassword`, `updatePassword`, `verifyOtp`. Uses `onAuthStateChange` for session tracking. Sign-out performs full cleanup (localStorage, sessionStorage, global scope). |

---

## 7. Hooks (`src/hooks/`)

### Data Hooks
| Hook | Purpose |
|------|---------|
| `useAccountsData` | Fetches accounts from Supabase with real-time updates |
| `useAccountsActions` | CRUD operations for accounts (create, update, archive, toggle visibility) |
| `useAccountDeletion` | Account deletion with confirmation logic |
| `useTransactionsData` | Transaction data with optimistic updates |
| `useBillsData` | Re-exports `useBillsManager` for convenience |
| `useBillsManager` | Orchestrates bills CRUD, state, and notifications |
| `useBillsCrud` | Raw Supabase CRUD for bills table |
| `useBillsState` | Local bill state management with optimistic updates |
| `useBillsNotifications` | Toast notifications for bill operations |
| `useBillsTable` | Table-specific state (sorting, filtering) |
| `useBillFormValidation` | Zod-based bill form validation |
| `useCategoriesData` | Categories CRUD with optimistic updates and sorting |
| `useInvestmentAccounts` | Investment accounts with portfolio value calculation |
| `useAllocations` | Asset allocation management |
| `useAssets` | Asset CRUD operations |
| `useAssetSearch` | Asset search with debouncing |
| `useProfileData` | User profile CRUD |
| `useProfileImageUpload` | Avatar image upload to Supabase Storage |
| `useReportsData` | Financial report data aggregation |
| `useSavingsGoals` | Savings goals CRUD |
| `useNotifications` | Local notification system (localStorage-based) |
| `useUserPreferences` | User preferences storage |

### Utility Hooks
| Hook | Purpose |
|------|---------|
| `useRealTimeUpdates` | Generic Supabase Realtime subscription hook |
| `useRealTimePrices` | Real-time asset price updates |
| `useOptimisticUpdates` | Optimistic update utilities |
| `usePasswordChange` | Password change flow |
| `useTheme` | Theme state (dark/light/system) |
| `useCategoryForm` | Category form state management |
| `use-mobile` | Mobile breakpoint detection |
| `use-toast` | Toast notification hook |
| `common.ts` | Shared hook utilities |

---

## 8. Pages (`src/pages/`)

| Route | Page Component | Description |
|-------|---------------|-------------|
| `/` | `Index.tsx` | **Dashboard** — greeting, balance summary, quick actions, main sections |
| `/accounts` | `Accounts.tsx` | **Accounts** — list, create, edit, transfer, archive |
| `/accounts/:id` | `Account.tsx` | Redirects to `/profile` (legacy route) |
| `/transactions` | `Transactions.tsx` | **Transactions** — list, search, filter, import |
| `/transactions/new` | `TransactionForm.tsx` | Create new transaction |
| `/transactions/:id` | `TransactionDetail.tsx` | Transaction detail view |
| `/transactions/:id/edit` | `TransactionForm.tsx` | Edit existing transaction |
| `/bills` | `Bills.tsx` | **Recurring** — manage recurring income/expenses, yearly projection |
| `/investments` | `Investments.tsx` | **Investments** — portfolio tracking, real-time prices |
| `/profile` | `Profile.tsx` | **Settings** — account, categories, notifications, integrations, reports (tabbed) |
| `/auth` | `auth/AuthPage.tsx` | **Auth** — login, register, forgot/reset password, verify email (view-based) |
| `/auth/callback` | `auth/AuthCallbackPage.tsx` | OAuth callback handler |
| `*` | `NotFound.tsx` | 404 page |

---

## 9. Types (`src/types/`)

| File | Types |
|------|-------|
| `accounts.ts` | `Account`, `Transfer` |
| `auth.ts` | `AuthUser`, `AuthContextType` |
| `bills.ts` | `Bill`, `NewBillData` |
| `categories.ts` | `Category`, `CategoryInfo` |
| `common.ts` | Shared utility types |
| `investments.ts` | `InvestmentAccount`, asset-related types |
| `notifications.ts` | `Notification`, `NotificationAction`, `NotificationPreferences` |
| `transactions.ts` | `Transaction`, `CategoryData` |
| `index.ts` | Re-exports all types |

---

## 10. Utilities (`src/utils/`)

| File | Functions |
|------|-----------|
| `authUtils.ts` | `cleanupAuthState()` — clears Supabase auth tokens; `isRememberMeEnabled()` |
| `categoryUtils.ts` | `useCategoryInfo()` — maps category IDs to names/icons/colors |
| `dateUtils.ts` | `formatTransactionDate()` — "Today", "Yesterday", or formatted date |
| `pwa.ts` | PWA registration and update utilities |

---

## 11. Edge Functions (`supabase/functions/`)

| Function | Purpose |
|----------|---------|
| `process-transaction-import/` | Processes uploaded bank statements. Supports **AI mode** (OpenAI GPT-4o-mini) and **manual mode** (basic CSV parsing). Fetches user's API key from `user_api_keys` table, matches categories, returns parsed transactions. |
| `update-asset-prices/` | Updates investment asset prices from external APIs |
| `populate-assets/` | Populates the assets table with available stocks/crypto/ETFs |

---

## 12. Database Schema (Supabase)

### Core Tables
| Table | Purpose | Key Columns |
|-------|---------|-------------|
| `profiles` | User profiles | `id` (FK to auth.users), `full_name`, `email` |
| `accounts` | Financial accounts | `id`, `user_id`, `name`, `balance`, `currency`, `account_type`, `color`, `icon`, `hide_balance`, `is_archived` |
| `transactions` | Financial transactions | `id`, `user_id`, `account_id`, `amount`, `type` (income/expense/transfer), `transaction_type` (manual/transfer/automated/imported), `description`, `date`, `category_id`, `photo_url`, `source_account_id`, `destination_account_id` |
| `categories` | Transaction categories | `id`, `user_id` (null = default), `name`, `type` (income/expense), `icon`, `color` |
| `bills` | Recurring bills/income | `id`, `user_id`, `name`, `amount`, `type`, `repeat_pattern`, `specific_day`, `account_id`, `category_id`, `is_active`, `include_in_forecast` |
| `investment_accounts` | Investment portfolios | `id`, `user_id`, `name`, `total_deposits`, `current_value` |
| `assets` | Tradable assets | `id`, `symbol`, `name`, `asset_type` (stock/crypto/etf), `current_price`, `price_updated_at`, `market_cap`, `volume_24h`, `price_change_24h` |
| `allocations` | Portfolio allocations | `id`, `investment_account_id`, `asset_id`, `percentage`, `initial_price`, `invested_amount`, `purchase_price`, `is_active` |
| `user_api_keys` | Encrypted API keys | `id`, `user_id`, `service_name`, `api_key_encrypted`, `is_active` |

### Database Functions
- `update_account_balance(account_id, amount_change)` — Atomically updates account balance
- `decrypt_api_key(encrypted_key)` — Decrypts stored API keys

---

## 13. Authentication Flow

1. **Entry:** User lands on any route → `ProtectedRoute` checks `useAuth()`.
2. **Unauthenticated:** Redirected to `/auth?view=login`.
3. **Login:** Email/password via `signInWithPassword()` or Google OAuth via `signInWithOAuth()`.
4. **Registration:** `signUp()` → email verification → redirect to `/auth?view=verify-email`.
5. **Session:** Managed by Supabase Auth with `onAuthStateChange`. Tokens stored in `localStorage`.
6. **Logout:** `signOut()` clears all auth state (localStorage, sessionStorage, Supabase global scope) → hard redirect to `/auth?view=login`.
7. **Password Reset:** `resetPasswordForEmail()` → email link → `/auth?view=reset-password` → `updateUser()`.

---

## 14. Design System

### Color Tokens (HSL in `index.css`)
- **Primary:** `142 76% 36%` (green)
- **Accent:** `142 86% 28%` (darker green)
- **Destructive:** `0 84.2% 60.2%` (red)
- **Warning:** `38 92% 50%` (amber)
- **Info:** `221 83% 53%` (blue)
- Full light/dark mode token sets
- Custom `--finapp-*` tokens for app-specific styling

### Component Classes
- `.card-hover` — Lift + shadow on hover
- `.gradient-card` / `.gradient-primary` — Gradient backgrounds
- `.status-indicator` / `.status-success` / `.status-warning` — Status badges
- `.auth-*` — Auth form styles (container, title, input, buttons, links)

### Spacing System
CSS custom properties: `--space-xs` through `--space-2xl` (0.25rem–3rem)

---

## 15. Key Patterns & Architecture Decisions

1. **Supabase as single source of truth** — All persistent data flows through Supabase. No mock data stores.
2. **Optimistic updates** — Categories, bills, and some transactions use optimistic state updates with rollback.
3. **Real-time subscriptions** — `useRealTimeUpdates` hook subscribes to Supabase Realtime for accounts/transactions.
4. **Soft delete for accounts** — Accounts are archived (`is_archived = true`), not deleted.
5. **Notification system** — Currently localStorage-based (not persisted to DB). Tracks bill due dates and monthly checkups.
6. **Investment valuation** — Three calculation methods: invested amount + purchase price, percentage-based with initial price, or fallback to deposits.
7. **Transaction import** — Multi-step wizard: upload → preview → choose AI/manual → process → review → import.
8. **Auth cleanup** — Aggressive cleanup on logout to prevent stale sessions.

---

## 16. Known Issues & Technical Debt

1. **Notification hook uses localStorage** — `useNotifications` references `mockAccounts` and `mockBills` keys. Should migrate to Supabase tables.
2. ~~**Mixed language in toasts**~~ — ✅ Fixed (2026-02-11): Standardized to English.
3. **Duplicate type definitions** — `AuthUser` and `AuthContextType` exist in both `src/types/auth.ts` and `src/contexts/AuthContext.tsx` with slight differences.
4. ~~**`bg-finapp-background` usage**~~ — ✅ Fixed (2026-02-11): Unified to `bg-background` across all pages.
5. **Account.tsx is a redirect shell** — `pages/Account.tsx` only redirects to `/profile`. Can be removed if route is dropped.

---

## 17. Environment & Configuration

- **Supabase URL:** `https://myrcukbpvjotrluvcige.supabase.co`
- **Supabase Anon Key:** Stored in `src/integrations/supabase/client.ts` (publishable)
- **Path Alias:** `@/*` → `./src/*` (configured in `tsconfig.json` and `vite.config.ts`)
- **TypeScript:** Strict null checks disabled, implicit any allowed (relaxed for rapid development)

---

## 18. Update Log

| Date | Changes |
|------|---------|
| 2026-02-11 | Initial documentation created. Full project audit completed. |
| 2026-02-11 | Fixed: Portuguese toasts → English, unified `bg-finapp-background` → `bg-background`, improved login network error handling, removed debug console.logs. |
