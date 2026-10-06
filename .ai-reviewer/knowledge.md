# new1 reviewer notes

## Architecture

This is a Vite-powered React 19/TypeScript dashboard application using React Router for page navigation, Tailwind CSS v4 alongside project-specific CSS classes, and Recharts for dashboard visualizations. Routing is centralized in `src/App.tsx`; pages compose reusable dashboard, profile, table, and UI components under `src/components`. Authentication and profile persistence are abstracted behind `src/lib/auth`, while the app shell uses browser storage and DOM events for theme and user updates.

## Conventions

- Use functional React components with typed props and named exports for reusable components, as in `src/components/dashboard/MetricCards.tsx` and `src/components/profile/ProfileCard.tsx`.
- Keep dashboard-specific components grouped by feature (`src/components/dashboard`, `src/components/profile`, `src/components/tables`) and shared primitives under `src/components/ui`.
- Prefer controlled form components. `src/components/profile/ProfileForm.tsx` receives `profile`, `onChange`, and `onSubmit`, and centralizes field updates through the `update` helper.
- Use `type` aliases for local data contracts and props, such as `MetricProps` in `src/components/dashboard/MetricCards.tsx` and `ProfileValues` in `src/components/profile/ProfileForm.tsx`.
- Use Lucide icons as React components and pass visual properties through props or styles; examples include `RefreshCw` in `MetricCards.tsx` and `LogOut` in `Navbar.tsx`.
- Route protection is implemented declaratively with an outlet wrapper: authenticated routes belong inside `ProtectedRoutes` in `src/App.tsx`; public login and signup routes remain outside it.
- Styling commonly combines semantic/project CSS class names with utility classes. For example, `Navbar.tsx` uses `topbar` and `account-links` while also using `flex`, `text-[28px]`, and related utilities.
- The `@/*` alias maps to `src/*` in `tsconfig.json` and `vite.config.ts`, although the sampled components predominantly use relative imports.

## Intentional non-standard choices

- Dashboard chart and table content is currently static module-level data (`lineData`, `pieData`, `salesData`, and `people`); do not assume every displayed value is backed by an API.
- Theme state is intentionally persisted in `localStorage` and applied through `document.documentElement.dataset.theme` in `src/components/dashboard/Navbar.tsx`.
- Profile changes are communicated across components with the custom `dashboard-user-updated` window event rather than a global state library (`Navbar.tsx`).

## Watch out for

- Verify route casing and link targets: `App.tsx` registers `/Orders` with a capital `O`, while `Sidebar.tsx` links to `/orders`; this can break navigation on case-sensitive routing.
- Check buttons for actual behavior when modifying dashboard UI. Task controls in `src/components/dashboard/Tasks.tsx` have labels and icons but no click handlers, and the mobile menu button in `Navbar.tsx` does not open navigation.
- Preserve controlled-input invariants in `ProfileForm.tsx`; first/last name edits intentionally derive the username, and the displayed username field is disabled.
- Confirm disabled-state intent in the profile form: the “Company (disabled)” field is rendered without passing `disabled`, unlike the username field.
- Avoid changing chart container sizing casually; Recharts components rely on `ResponsiveContainer` inside feature-specific wrapper elements such as `.line-chart-wrap` and `.sales-chart-wrap`.