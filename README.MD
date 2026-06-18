# Skill API Marketplace: A Self-Hosted, AI-Powered B2B Multi-Tenant Code Directory

An enterprise-grade, privacy-first B2B monetization ecosystem that processes complex code pipelines, utilizes a local 1.5-Billion parameter LLM for structural architectural parsing, manages secure multi-tenant data masking, and provides automated tech-stack asset tracking over a globally scalable cloud database.

---

## 🏗️ Core System Architecture

The platform architecture is designed to enforce strict security boundaries, eliminating cross-origin dependency vulnerabilities and ensuring that premium developer identities and raw profile links remain unmasked until explicit ledger authorization rules are met.
### 1. Data Processing & Analytical Core
* **Database Infrastructure:** Powered by a cloud PostgreSQL schema hosted on Supabase, handling relational data mapping for distinct user groups, developer assets, and recruiter transaction tokens.
* **Local AI Parsing Engine:** Bypasses external cloud API rate boundaries by executing a dedicated 1.5-Billion parameter language model (`Qwen2.5-Coder-1.5B-Instruct`) natively within the notebook container memory. It automatically extracts technology tags, detects languages, and drafts structural summaries using deterministic sampling boundaries (`temperature=0.1`).
* **Native Communication Layer:** Uses a streamlined `ipywidgets` dashboard abstraction to directly coordinate memory queues with the FastAPI backend worker, bypassing cross-origin iframe security boundaries (`Failed to Fetch`).

### 2. Multi-Tenant Monetization & Security Engine
* **State Tracking Isolation:** Tracks discrete recruiter sessions independently. If a recruiter ID lacks an active transaction token matching a specific developer profile UUID inside the `profile_unlocks` ledger table, a high-security visual mask replaces the personal data.
* **Asset Masking Profiles:** Dynamically replaces restricted database records with placeholder security templates (`🔒 Locked`) at the API layer, blocking network snooping or unauthorized credential leaks.

---

## 📊 Database Schema Relationships

The platform coordinates transactional tracking across three primary entity structures:

```sql
-- 1. Profiles Table: Stores baseline developer access metadata
CREATE TABLE public.profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    full_name TEXT NOT NULL,
    github_url TEXT,
    linkedin_url TEXT,
    portfolio_url TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 2. Pipelines Table: Tracks functional deployed engineering code loops
CREATE TABLE public.pipelines (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    developer_id UUID REFERENCES public.profiles(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    description TEXT,
    primary_language TEXT NOT NULL,
    tech_stack TEXT[] DEFAULT '{}'::TEXT[],
    ai_summary TEXT NOT NULL,
    is_verified BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 3. Profile Unlocks Table: Multi-tenant ledger mapping authorization keys
CREATE TABLE public.profile_unlocks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    recruiter_id UUID NOT NULL,
    developer_id UUID REFERENCES public.profiles(id) ON DELETE CASCADE,
    unlocked_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    UNIQUE(recruiter_id, developer_id)
);
