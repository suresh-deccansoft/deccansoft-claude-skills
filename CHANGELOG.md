# Changelog

All notable changes to the plugins are documented here.

## fullstack-project-setup 1.2.0 — 2026-09-16

New locked architecture decision #16 — backend architecture standardization with ArchUnitPython:

- **ArchUnitPython (`archunitpython>=1.7.0`) integration**: Architecture test platform added to Python backend test suite (`backend/tests/test_architecture.py`), executed natively via `pytest`.
- **Zero cycle detection**: `project_files("app/").should().have_no_cycles()`.
- **Layer boundary enforcement**: `app.core` infrastructure is strictly prevented from depending on domain feature slices in `app.features`.
- **1000-line metric assertion**: `metrics("app/").count().lines_of_code().should_be_below(1000)` running natively in pytest with full diagnostics.
- Templates updated: `backend/pyproject.toml`, `backend/tests/test_architecture.py`, `root/CLAUDE.md.template`.

## fullstack-render-web-setup 1.0.0 — 2026-09-16

Initial release of Render-focused full-stack web scaffolding plugin:

- **Render deployment blueprint (`render.yaml`)**: Preconfigured for Render Web Service (frontend), Render Web Service (FastAPI), and Render Managed PostgreSQL (`pgvector`, `pgcrypto`).
- **Web-only stack**: Removed all React Native / mobile dependencies (`apps/native`, Metro, Expo, NativeWind, Babel, `jest-expo`, `EXPO_PUBLIC_*`).
- **ArchUnitPython integration**: Full architecture testing guardrails included out of the box.
- **Render database URL handling**: Automatic normalization of Render's `postgres://` / `postgresql://` connection strings to `postgresql+asyncpg://`.

## fullstack-project-setup 1.1.0 — 2026-09-10

New locked architecture decision #15 — styling:

- **Tailwind CSS (v4, `@tailwindcss/vite`) for web styles, NativeWind for
  native styles.** One Tailwind class vocabulary on both platforms; no CSS
  modules/CSS-in-JS on web, no `StyleSheet.create`/inline style objects for
  layout in native screens. Shared packages never style anything. This does
  not reopen the rejected universal-UI decision — NativeWind is the native
  app's styling DX only.
- Web templates: Tailwind v4 wired into `vite.config.ts`, `package.json`;
  new `src/styles.css` (`@import "tailwindcss"`), imported in `main.tsx`;
  reference todos page styled with Tailwind classes.
- Native templates: NativeWind v4 (`nativewind`, `react-native-reanimated`,
  `react-native-safe-area-context`, `tailwindcss` v3 dev dep) with
  `babel.config.js`, `metro.config.js`, `tailwind.config.js`, `global.css`,
  `nativewind-env.d.ts`; new `index.ts` entry (imports `global.css`,
  `registerRootComponent(App)` — closes the gap where `package.json`
  declared `main: "index.ts"` with no such file); reference screen uses
  `className`.
- `root/CLAUDE.md.template`: new hard rule #11 so every scaffolded repo
  carries the styling rule.
- Tailwind majors diverge per platform (web v4, native v3 per NativeWind v4)
  — documented under decision #14 (per-platform version divergence).

## openclaw-azure 1.0.0 — 2026-05-24

Initial release.

## 1.0.0 — 2026-05-24

Initial release.

- `/openclaw-azure:deploy` guided wizard: preflight → collect inputs → BuildKit image build in ACR →
  persistence choice → deploy → verify.
- Two persistence strategies, both automated:
  - **local + periodic Standard-SMB backup** (`deploy-sync.sh`) — cheap, ≤5-min crash-loss window.
  - **premium Azure Files NFS + VNet** (`deploy-nfs.sh`) — real-time durability.
- `openclaw-azure-doctor` agent for diagnosing silent/broken deployments from the logs.
- Reference docs: Azure Files SMB hard-link limitation, persistence comparison, and the full
  gotchas list (BuildKit, IPv6 egress, `gateway.mode`, single-replica `getUpdates` conflict).
- Bundled scripts use `${CLAUDE_PLUGIN_ROOT}`, run `az` sequentially, and are idempotent
  (create-or-update).
