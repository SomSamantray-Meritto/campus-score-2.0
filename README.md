# Campus Score 2.0

AI-powered visibility tracker that measures how prominently an institution appears across AI-generated search responses.

## What it does

Enter any institution name → the tool runs 121 AI-generated queries across 11 topics → calculates a visibility score based on how often and how highly the institution is ranked in AI answers.

**Report includes:**
- Overall visibility score + average rank
- Per-topic breakdown (bar + pie charts)
- Every query with the full AI answer and brand mentions
- Competitor landscape (who else appears in your queries)
- Source domains cited by AI

## Tech Stack

- **Frontend**: Next.js 16, React 19, Tailwind CSS, shadcn/ui, Recharts
- **Database**: Supabase (PostgreSQL + Realtime)
- **AI**: OpenAI `gpt-5-nano` (web search enabled) + Perplexity `sonar`
- **Deployment**: Vercel

## Setup

### 1. Clone and install

```bash
git clone https://github.com/SomSamantray-Meritto/campus-score-2.0
cd campus-score-2.0
npm install
```

### 2. Environment variables

Create `.env.local` in the project root:

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
OPENAI_API_KEY=your_openai_key
PERPLEXITY_API_KEY=your_perplexity_key
```

### 3. Database setup

Run these SQL scripts in order in the **Supabase SQL Editor**:

1. `supabase-schema.sql`
2. `database-migration-weighted-visibility.sql`
3. `database-migration-add-institution-mention.sql`

Then go to **Database → Replication** and enable Realtime for `analyses` and `queries` tables.

### 4. Run locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## How it works

1. **Perplexity** generates 11 topics + 11 queries per topic (121 total) tailored to the institution
2. **OpenAI** (with web search) answers each query and extracts brand mentions
3. Visibility score = weighted rank position (Rank 1 = 100%, 2-3 = 50%, 4-5 = 25%, 6+ = 10%)
4. Results stream into Supabase in real time as queries complete
