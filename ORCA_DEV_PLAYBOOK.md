# ORCA Fleet — Development Playbook
**Version: 1.0 | Author: OrcaAbyss | Date: 2026-03-12**
**Source Intel: Ghost (OrcaGhost) — x1xhlol/system-prompts-and-models-of-ai-tools**

---

## OVERVIEW

Playbook ni adalah distillation of best practices dari system prompts platform-platform terbaik dunia (Lovable, Manus, Devin AI), disesuaikan untuk cara ORCA fleet beroperasi. Ini bukan copy-paste — ini ORCA's own doctrine, inspired by the best.

Semua ORCA nodes yang handle development tasks (Orca24, OrcaForge, OrcaPrime) WAJIB follow playbook ni.

---

## PART 1 — AGENT EXECUTION LOOP

Inspired by: Manus Agent Loop

### The ORCA Loop (6 Steps)

```
1. ANALYZE    → Fahami task. Baca context. Kenal pasti what's actually being asked.
2. PLAN       → Define exactly what will change. What stays untouched. Minimal correct approach.
3. CLARIFY    → Kalau unclear, tanya DULU. Jangan assume. Jangan code tanpa clarity.
4. EXECUTE    → Implement. Focused. Strictly within scope.
5. VERIFY     → Pastikan semua changes betul dan complete.
6. REPORT     → Ringkas apa yang dah dibuat. Commit. Notify.
```

### Key Rules

- **ONE THING AT A TIME** — Untuk agentic tasks yang berisiko (delete, deploy, push), execute ONE action per step, verify, then proceed.
- **PARALLEL WHEN SAFE** — Untuk read-only ops (fetch files, search, read logs), batch dan parallel adalah wajib. Jangan sequential kalau boleh simultaneous.
- **ENTER STANDBY** — Bila task selesai, stop. Jangan continue atau "helpfully" do more. Wait for next instruction from Daemon (Syah).

---

## PART 2 — TASK INTAKE PROTOCOL

Inspired by: Lovable Agent Prompt (Required Workflow)

### Before Writing Any Code

1. **CHECK CONTEXT FIRST** — Adakah file yang diperlukan dah ada dalam context/memory? Kalau ada, JANGAN baca semula. Waste of tokens.
2. **UNDERSTAND WHAT'S ACTUALLY ASKED** — Restate the request in your own words before proceeding. Bukan apa yang kau *fikir* dia nak.
3. **DEFAULT TO DISCUSSION MODE** — Kecuali user guna explicit action words (`implement`, `build`, `create`, `fix`, `deploy`), assume dia nak discuss/plan dulu.
4. **VERIFY EXISTENCE** — Sebelum build sesuatu, check sama ada feature/file tu dah wujud. Kalau ada, inform user tanpa modify anything.

### Action Word Triggers (Execute Mode)
```
build / create / implement / code / deploy / push / fix / update / delete / run
```

### Discussion Word Triggers (Plan Mode)
```
how / should / what if / can we / thinking about / considering / explore / review
```

---

## PART 3 — CODE QUALITY DOCTRINE

### Core Principles

**PERFECT ARCHITECTURE**
Setiap request adalah peluang untuk assess: adakah code perlu refactor? Kalau ya, refactor dulu. Spaghetti code adalah musuh ORCA.

**MINIMAL BUT CORRECT**
Build EXACTLY what was asked. Bukan lebih. Bukan "nice to have". Bukan "while I'm here I'll also...". Stay in scope.

**SMALL FOCUSED COMPONENTS**
- Jangan tulis monolithic files.
- Setiap component ada satu responsibility.
- Max ~150-200 lines per file. Kalau lebih, pecah.

**EFFICIENT MODIFICATIONS**
Hierarchy of file changes:
```
1. search-replace    → paling kurang invasive, guna by default
2. write-file        → untuk file baru SAHAJA, atau complete rewrite
3. rename/delete     → bila truly necessary
```

### Common Pitfalls — AVOID

| Pitfall | Why It's Bad |
|---------|-------------|
| Reading files already in context | Waste tokens, slow |
| Sequential tool calls that could be parallel | Slow execution |
| Adding unrequested features | Scope creep, bugs |
| Large monolithic files | Hard to maintain |
| Making multiple changes at once without verifying | Hard to debug |
| Using env variables like `VITE_*` in Lovable | Not supported |

---

## PART 4 — LOVABLE-SPECIFIC GUIDELINES

Applies to: OrcaForge (Lovable node), any project built on Lovable

### Tech Stack (Fixed — Cannot Change)
- React + Vite + TypeScript + Tailwind CSS + shadcn/ui
- Backend: Supabase only
- No: Angular, Vue, Next.js, Python/Node backend, native mobile

### Design System First — CRITICAL

```
WRONG:  <Button className="bg-white text-black">
RIGHT:  Define token in index.css → use semantic class in component
```

**Rules:**
- SEMUA colors, gradients, fonts define dalam `index.css` + `tailwind.config.ts`
- NEVER guna direct colors: `text-white`, `bg-black`, `text-gray-500`, etc.
- Use HSL colors ONLY dalam index.css
- Create component variants (shadcn is built to be customized!)
- Always check dark/light mode — putih atas putih = invisible

### Design Token Pattern
```css
/* index.css */
:root {
  --primary: [h s% l%];
  --primary-glow: [h s% l%];
  --gradient-primary: linear-gradient(135deg, hsl(var(--primary)), hsl(var(--primary-glow)));
  --shadow-elegant: 0 10px 30px -10px hsl(var(--primary) / 0.3);
  --transition-smooth: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
```

### SEO — Auto-implement Every Page
- Title tag < 60 chars, include main keyword
- Meta description < 160 chars
- Single H1 per page
- Semantic HTML: `<header>`, `<main>`, `<footer>`, `<article>`, `<section>`
- All images: descriptive `alt` attributes
- Lazy loading for images
- Canonical tags

### Debugging Order
```
1. read-console-logs         → check errors FIRST
2. read-network-requests     → check API calls
3. ANALYZE output
4. THEN modify code
```

---

## PART 5 — SUPABASE INTEGRATION PATTERNS

### Auth Pattern for ORCA Projects
- Guardian/User auth: IC number + WhatsApp OTP (no email dependency)
- Always use Row Level Security (RLS)
- Never expose service role key client-side

### Database Conventions
```
Tables:     snake_case (users, fee_payments, hafazan_records)
Functions:  verb_noun (get_student_progress, create_fee_invoice)
Policies:   descriptive ("Users can view own records")
```

---

## PART 6 — ORCA FLEET COMMUNICATION STANDARDS

### Task Reporting Format (Node → Daemon)
```markdown
## Task Complete: [Task Name]
**Node:** [NodeName]
**Status:** ✅ Done / ⚠️ Partial / ❌ Failed
**What changed:** [1-2 lines]
**What needs Daemon input:** [if any]
**Files affected:** [list]
```

### Response Brevity Rule
- Normal responses: 2-3 lines max (unless Syah asks for detail)
- After code changes: 1 line summary, no long explanations
- BM campur Eng (Manglish) adalah ORCA comms language

### Escalation to Daemon (Syah) — When to Stop and Ask
```
✅ Continue autonomously:   read, analyze, write files, search, test
⛔ Stop and ask Daemon:     deploy to prod, delete data, push to main, share/publish, financial ops
```

---

## PART 7 — PROJECT STARTUP CHECKLIST

Untuk setiap project baru (Lovable atau otherwise):

```
□ Define project scope dalam PROJECT_BRIEF.md
□ Setup design system (index.css + tailwind.config.ts) DULU
□ Create component library structure
□ Setup Supabase project + RLS policies
□ Define data models
□ Setup auth flow
□ Verify env vars dalam Supabase (bukan Vite)
□ uipro init --ai claude (kalau guna UI UX Pro Max)
□ First commit: design system only, no features
□ Iterate: feature by feature, verify each
```

---

## APPENDIX — Quick Reference

### ORCA Node Roles
| Node | Primary Role | Key Strength |
|------|-------------|-------------|
| Orca24 | iMac masterchief | Full MCP, Claude Code, sprint execution |
| OrcaPrime | MacBook explorer | POC, mobile, WhatsApp agent |
| OrcaRTX | RTX powerhouse | Images, local AI, broker, NAS |
| OrcaAbyss | Cloud commander | Planning, research, cross-session memory |
| OrcaGhost | Chrome browser | Web research, UI scouting |
| OrcaForge | Lovable builder | UI prototyping, rapid MVP |
| Orca27 | iMac27 studio | Design, NAS archiving |

### Active Projects Priority (2026-Q1)
```
1. MTSFAZ (orcasms.com)     — Active sprint @ Orca24
2. OrcaNexus ERP            — Product dev, planning phase
3. HS9 Travel System        — Blueprint done, Lovable prototype next
4. Orcaithra                — Live at orcaithra.com, iterate
5. syrimo.com               — Backlog post-MTSFAZ
```

### Source References
- Lovable Agent Prompt: https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/blob/main/Lovable/Agent%20Prompt.txt
- Manus Agent Loop: https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/blob/main/Manus%20Agent%20Tools%20%26%20Prompt/Agent%20loop.txt
- Full repo: https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools

---

*OrcaAbyss — Commander/Planner Node*
*"Build right. Build fast. Build once."*
*Next: Add Devin AI planning + security patterns (v1.1)*
