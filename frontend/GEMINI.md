# 🏛️ MANDATORY ARCHITECTURAL, DESIGN SYSTEM & DEVELOPMENT RULES

**Governing Authorities (Must be read and strictly enforced before any project development or code modification):**
1. [WEB_FRONTEND_ARCHITECTURE_AND_DEVELOPMENT_RULES_V1.md](file:///home/pawan/Desktop/Librarysoftware%20testing/temp_test/frontend/Ai_Web_Mobile_project_development_instruction_documentation/WEB_FRONTEND_ARCHITECTURE_AND_DEVELOPMENT_RULES_V1.md)
2. [WEB_FRONTEND_UI_UX_DESIGN.md](file:///home/pawan/Desktop/Librarysoftware%20testing/temp_test/frontend/Ai_Web_Mobile_project_development_instruction_documentation/WEB_FRONTEND_UI_UX_DESIGN.md)
3. [FRONTEND_ROLE_MODULE_CREATE_AUDIT_REPAIR_V2_11.md](file:///home/pawan/Desktop/Librarysoftware%20testing/temp_test/frontend/Ai_Web_Mobile_project_development_instruction_documentation/FRONTEND_ROLE_MODULE_CREATE_AUDIT_REPAIR_V2_11.md)

---

## 1. Master Cross-Document Precedence Order
When instructions conflict across documents or user requests, apply this exact hierarchy of authority:
1. **Security, privacy, legal, authorization, tenant isolation, and data protection**
2. **Accessibility and browser platform safety requirements (WCAG 2.2 AA)**
3. **Explicit user-approved scope and verified API contracts**
4. **`WEB_FRONTEND_ARCHITECTURE_AND_DEVELOPMENT_RULES_V1.md` (Normative Architecture Authority)**
5. **`WEB_FRONTEND_UI_UX_DESIGN.md` (Visual, Color & Design Token Decisions)**
6. **`FRONTEND_ROLE_MODULE_CREATE_AUDIT_REPAIR_V2_11.md` (Audit Sequencing, Scaffolding, Delivery Artifacts)**
7. **Sub-module documentation trio (`_features.md`, `_forbidden.md`, `_theme_contract.md`)**
8. **Existing implementation conventions**

*A lower-priority rule MUST NOT override a higher-priority rule. Never resolve a conflict through speculative invention.*

---

## 2. Core Architectural Invariants & Laws

### 2.1 Anti-Destruction & Zero Feature Eviction Law
- **Strict Prohibition:** The AI MUST NOT delete, remove, comment out, or strip away existing features, UI elements, buttons, form fields, modals, or business flows simply because they are half-built, incomplete, or causing type/test errors. Deleting half-built features to get a "clean audit pass" is strictly prohibited as destructive malpractice.
- **Mandatory Additive Completion:** The AI MUST actively COMPLETE the missing half: add missing types, wire up API queries/mutations, implement missing Zod schemas, author missing loading skeletons, empty states, error boundaries, connect handlers, and correct syntax or architectural violations until the feature is 100% functional and architecture-compliant.

### 2.2 No Crude Browser Alerts — Mandatory `useConfirm` Dialogs
- `window.alert()`, `window.confirm()`, and single-click destructive deletes are strictly prohibited in production code.
- Destructive or irreversible actions MUST use accessible, luxury modal dialogs (`useConfirm()` / Radix-based `<Dialog>`) with:
  - High-contrast title and explanation of consequences.
  - Distinct Danger button variant.
  - Cancel option that restores focus.
  - Focus trapping and Escape key listener.

### 2.3 Strict Form Completeness & Zod Validation
- All forms MUST use **React Hook Form v7** + **Zod v3** schemas.
- Forms must map server 400 validation error responses back to individual field error states (`fieldError` inline helpers).
- Form fields must declare programmatic `aria-invalid` and `aria-describedby` linking to error alert elements (`role="alert"`).
- Required fields MUST declare programmatic `aria-required="true"`.

### 2.4 Canonical 14-Folder Module Directory Structure
Every feature module MUST strictly adhere to the module directory layout:
```text
<role>_<domain>/<submodule>/
├── api/              # Module-owned API contracts & query-key factories
├── components/       # Feature-specific components, dialogs, and sections
├── config/           # Route constants, URL configs, and page-size options
├── constants/        # Fixed operational options, status maps, and labels
├── contracts/        # TypeScript host contract stubs (*.d.ts)
├── hooks/            # Custom React hooks (state, url-query, debounce)
├── locales/          # Localization JSON files (_en.json, _hi.json)
├── mocks/            # Isolated test fixtures and mock handlers (NEVER imported in prod)
├── schemas/          # Zod validation schemas
├── state/            # Local or Zustand store (only when cross-component justified)
├── tests/            # Vitest unit, regression, and harness tests
├── types/            # TypeScript interfaces and component prop types
├── utils/            # Pure utility functions, formatters, and sanitizers
├── <module>_features.md      # Feature capabilities inventory
├── <module>_forbidden.md     # Architecture boundaries and prohibited patterns
├── <module>_theme_contract.md# Token contracts and CSS class mappings
└── page.tsx          # Next.js route entrypoint
```

### 2.5 Fail-Closed Transport & Idempotency Key Enforcement
- All mutating HTTP methods (`POST`, `PUT`, `PATCH`, `DELETE`) MUST enforce client-generated UUIDv4 idempotency keys via `createIdempotencyKey()`.
- Network and API failures MUST fail closed: display descriptive error states, never invent persisted success, and prevent ghost records.

---

## 3. Global Design System & Token Authority (`WEB_FRONTEND_UI_UX_DESIGN.md`)

### 3.1 Theme Scopes & Accent Palette
- **Role Theme Containers:**
  - Manager: `.mgr-theme`
  - Admin: `.admin-theme`
- **Primary Brand Accent:** `#FACC15` (Yellow-400 / Gold)
- **Dark Mode Root Surfaces:**
  - Background Root: `#050505` (Deep Neutral Black)
  - Card & Floating Surfaces: `#111111` / `#18181B`
  - Elevated Popovers & Dialogs: `#1E1E24`
  - Borders: `rgba(255, 255, 255, 0.08)` to `0.15`

### 3.2 WCAG 2.2 AA Mandatory Standards
- **Non-Color Indicators (WCAG 1.4.1):** Color alone MUST NEVER be the sole conveyor of information. All status badges and alerts must include explicit textual labels or distinct geometric icon shapes (`CheckCircle` for Active, `AlertTriangle` for Pending, `Ban` for Suspended).
- **Minimum Touch Targets (WCAG 2.5.8):** All interactive buttons and touch controls MUST maintain at least 44x44 CSS pixels (`min-h-11 min-w-11` or minimum 44px height).
- **Focus Indicators (WCAG 2.4.7):** Every interactive control MUST display a prominent focus indicator:
  `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2`.
- **Motion Reduction (WCAG 2.3.3):** All CSS transitions and animations must be wrapped with Tailwind's `motion-safe:` utility to respect `prefers-reduced-motion`.

