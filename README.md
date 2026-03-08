# orca-lovable-template
OrcaForge Repo

# 🔨 orca-lovable-template
> ORCA Fleet — Standard Lovable Project Template  
> Maintained by Orca24 | Architected by OrcaAbyss

---

## UNTUK ORCAFORGE (Lovable)

Kamu adalah **OrcaForge** — UI Builder Node dalam ORCA Fleet.  
Baca `ORCA_CONTEXT.md` untuk full identity dan fleet context sebelum mula kerja.

**Stack standard:**
- React + Vite + TypeScript
- Tailwind CSS + Shadcn/ui
- React Router v6
- Supabase (connect ke project Syah — bukan Lovable built-in)

**First steps dalam project baru:**
1. Baca `ORCA_CONTEXT.md` dan `PROJECT_BRIEF.md`
2. Setup routing untuk semua roles
3. Guna dummy data dari `/src/data/` dulu
4. Jangan connect Supabase lagi — prototype phase, UI flow sahaja
5. Pastikan mobile-responsive dari awal

---

## UNTUK ORCA24 (Production Handoff)

Ni repo yang OrcaForge dah kerjakan. Structure dah clean dan ready untuk production sprint.

### Sebelum start, check:
- [ ] `PROJECT_BRIEF.md` — requirements dan scope
- [ ] `ORCA_CONTEXT.md` — project context dan modul list
- [ ] `/src/types/` — TypeScript interfaces (OrcaForge dah define)
- [ ] `/src/data/` — dummy data (jadikan reference untuk Supabase schema)
- [ ] `.env.example` — semua env variables yang diperlukan

### Setup local:
```bash
git clone [repo-url]
cd [project-name]
npm install

# Copy env file
cp .env.example .env.local
# Fill in Supabase credentials Syah dalam .env.local
```

### Connect Supabase:
```bash
# Install Supabase CLI
npm install -g supabase

# Link ke project Syah
supabase link --project-ref [PROJECT_REF]

# Apply migrations
supabase db push
```

### Supabase client dah configured:
```
src/lib/supabase.ts — tinggal isi .env sahaja
```

### Production checklist sebelum deploy:
- [ ] Replace semua dummy data dengan real Supabase queries
- [ ] Enable RLS pada semua tables
- [ ] Implement proper auth guards
- [ ] Error boundaries pada semua pages
- [ ] Loading + empty states verified
- [ ] Mobile responsive check
- [ ] Security scan (guna OrcaForge scanner dulu)
- [ ] Environment variables configured kat Vercel

---

## PROJECT STRUCTURE

```
src/
├── components/       # Reusable UI components (PascalCase)
├── pages/            # Route-level pages
├── data/             # Mock data — reference untuk Supabase schema
├── hooks/            # Custom React hooks
├── types/            # TypeScript interfaces — shared antara nodes
├── constants/        # App constants
├── lib/
│   └── supabase.ts   # Supabase client
└── context/          # React context providers
```

---

## FLEET CONTEXT

```
OrcaAbyss  → Commander — bagi architecture direction
OrcaForge  → Buat prototype dalam repo ni (Lovable)
Orca24     → Takeover untuk production sprint
```

**Communication:** Semua fleet updates via GDrive OrcaNet Broker  
**Abyss output folder:** GDrive folder `OrcaAbyss/`  
**Shared memory:** GDrive folder ID `1IKWLRRPNpO05gYZaNRyKjLzQdD7bjaEh`

---

## HANDOFF PROTOCOL

Bila OrcaForge siap prototype dan Syah dah approve:

1. OrcaForge commit semua changes dengan message: `feat: prototype complete — ready for Orca24`
2. Orca24 pull latest
3. Orca24 buat branch baru: `production/[project-name]`
4. Replace dummy data satu-satu dengan Supabase queries
5. Deploy ke Vercel bila ready

---

*Template ini diselenggara oleh Orca24*  
*Diarkitek oleh OrcaAbyss | ORCA Fleet | Sutera Nusantara Sdn Bhd*  
*"Build fast, build right, build ORCA." 🔨*
