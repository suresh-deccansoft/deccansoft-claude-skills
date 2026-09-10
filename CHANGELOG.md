# Changelog

All notable changes to the plugins are documented here.

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
