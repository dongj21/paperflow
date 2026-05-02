# Academic Paper Writing Project (v0.6.0)

## Research Configuration
**Topic:** [INSERT YOUR SPECIFIC RESEARCH TOPIC]
**Target Journal:** [INSERT TARGET JOURNAL]
**Study Design:** [RCT / Cohort / Case-Control / Case Series / Meta-analysis / etc.]

> ⚠️ Update this section for each new paper project

---

## Project Structure

```
project/
├── CLAUDE.md                     # Core rules & config (this file, auto-loaded)
├── docs/                         # Reference guides (read on demand)
│   ├── writing_guide.md          # Section-by-section writing guide
│   ├── expert_roles.md           # Expert team roles
│   ├── checklist_guide.md        # STROBE/CONSORT/PRISMA/CARE checklists
│   ├── qc_guide.md               # QC procedures
│   ├── statistical_analysis_guide.md
│   ├── evidence_guide.md         # Evidence 작성 가이드
│   ├── revision_guide.md         # Reviewer response guide
│   ├── figure_guide.md           # Figure generation guide
│   └── docx_guide.md             # DOCX 변환 가이드
├── knowledge/
│   ├── evidence.md               # 참고문헌 요약 자료집
│   ├── pdf/                      # Original PDFs (author_year_keyword.pdf)
│   └── summaries/                # 핵심 논문 상세 요약
├── data/
│   ├── raw_data.csv              # Original dataset (CSV/XLSX)
│   ├── analysis_plan.md          # Required before analysis (Rule 7)
│   └── py/                       # Analysis scripts (01_descriptive.py 등)
├── results/                      # Analysis output CSVs
├── drafts/
│   ├── draft_plan.md             # Required before drafting (Rule 8)
│   ├── 00_cover_letter.md
│   ├── 01_title.md ~ 09_figure_legends.md
│   ├── table_*.md
│   └── figures/
├── scripts/search_pubmed.py      # PubMed 검색 (no external deps)
├── review/qc_log.md              # QC round tracking
└── output/                       # Final compiled DOCX (title page / manuscript / tables)
```

**Multi-paper variant** — when one dataset → multiple papers:
- Add `paper{N}_{keyword}/` subfolder under `data/`, `results/`, `drafts/`, `review/`, `output/`
- `docs/`, `knowledge/`, `scripts/` stay shared (no subfolder)
- Each paper gets its own `analysis_plan.md` and `draft_plan.md`
- Original raw data at `data/`, paper-specific filtered data inside subfolder
- 서브폴더 네이밍: `paper{N}_{keyword}` (저자 지정 우선)

**Revision variant** — after reviewer comments:
- `drafts/revision/REV{N}/` and `output/revision/REV{N}/` (or under `paper{N}/` for multi-paper)
- Save only modified sections with `_REV{N}` suffix
- `review/reviewer_comments_REV{N}.md` for the original comments
- Response letter goes in the same revision folder

---

## Critical Rules (MUST FOLLOW)

### 1. Citation Integrity

- **NEVER fabricate or hallucinate references**
- **ALWAYS check `knowledge/evidence.md` first** before searching (avoid duplicate work)
- **New reference workflow** (상세: `docs/evidence_guide.md`):
  1. Search → verify paper exists
  2. Save PDF to `knowledge/pdf/author_year_keyword.pdf`
  3. Register in `knowledge/evidence.md` with summary & key points
  4. 핵심 논문은 `knowledge/summaries/`에 상세 요약 추가
  5. Then cite in manuscript

### 2. Redundancy Prevention

**Section content boundaries:**

| Section | Contains | Does NOT Contain |
|---------|----------|------------------|
| Introduction | Background, gap, rationale | Your results interpretation |
| Discussion | Your findings interpretation | Repeated background info |
| Results (text) | Narrative of findings | Exact numbers from tables |
| Tables | All numerical data | Narrative interpretation |

**Avoid triple duplication** — same data in Results 본문 + Table + Figure 모두 등장 금지:
- 정확한 수치 필요 → Table only
- 추세/분포 강조 → Figure only
- Results 본문은 Table을 짧게 reference
  - ✅ "Group A showed significantly better outcomes (Table 2, *p*=0.023)"
  - ❌ "Mean age was 54.3±12.1 years in Group A and 52.1±11.8 in Group B..."
- Table vs Figure 결정 시 사용자에게 물어봄 (둘 다 만들지 않음)

**Standard table structure:**

| Table # | Content |
|---------|---------|
| Table 1 | Baseline Characteristics |
| Table 2 | Main Results (primary + key secondary) |
| Table 3+ | Additional Analyses (subgroup, regression) |

> 가급적 5개 이하 권장. 부수적 분석은 Supplement로 분리. 단, 논문 흐름상 필수면 5개 초과 가능.

### 3. Consistency Requirements
**Abstract ↔ Methods ↔ Results ↔ Tables** 사이에 다음이 일치해야 함:
- Patient/sample numbers
- Statistical values (p-values, CIs, means, SDs)
- Time periods and follow-up duration
- Outcome measure names and definitions

### 4. QC Process (MANDATORY)
- Run **minimum 3 QC rounds** before submission (6 recommended)
- Procedures: `docs/qc_guide.md`
- Document in `review/qc_log.md`

### 5. File Versioning

**기본 규칙:** 저자가 별도 스타일을 지정하지 않으면 **날짜(YYMMDD)** 사용. 패턴: `{내용}_{버전}_{날짜}.{확장자}`

| 형식 | 용도 | 예시 |
|------|------|------|
| `_YYMMDD` | 기본 (날짜 기반) | `manuscript_260414.docx` |
| `_v1`, `_v2` | 저자 요청 시 | `manuscript_v1.docx` |
| `_REV1`, `_REV2` | Revision 제출본 | `manuscript_REV1_260414.docx` |
| `_FINAL` | 최종 제출본 | `manuscript_FINAL_260414.docx` |

- Phase 7 최초 제출본: 날짜 또는 버전 부여
- Revision: `_REV{N}` 표기 필수 (+ 날짜 병기 권장)
- 대규모 변경: 새 버전으로 저장 (덮어쓰지 않음)
- Minor 수정: 동일 파일명 (git으로 추적)

### 6. Multi-Paper Organization

> 하나의 데이터에서 여러 논문 작성 시 반드시 서브폴더로 분리

- `data/`, `results/`, `drafts/`, `output/`, `review/` 각각에 `paper{N}_{keyword}/` 생성
- `docs/`, `knowledge/`, `scripts/`는 공유 (서브폴더 불필요)
- 원본 데이터는 `data/` 루트, 논문별 필터링 데이터는 서브폴더
- Revision 시 각 논문 서브폴더 안에 `revision/REV{N}/` 생성, 수정된 섹션만 저장

### 7. Analysis Plan Mandatory

> **통계 분석 전 반드시 `analysis_plan.md`를 작성하고 사용자 승인 후 진행**

- **NEVER run statistical analysis without first creating `analysis_plan.md`**
- analysis_plan.md가 없으면 분석 스크립트 생성을 거부
- 같은 데이터라도 논문마다 연구 질문·대상·분석이 다르므로 **반드시 별도 작성** (Multi-paper: `data/paper{N}/analysis_plan.md`)
- 공유 데이터(`data/raw_data.csv`)에 대한 공통 plan은 만들지 않음

**필수 포함 내용:**
1. 연구 질문 및 가설
2. 대상 선정/제외 기준
3. 변수 정의 (primary/secondary/exploratory endpoints)
4. 통계 검정법 선택 및 근거
5. 유의수준 및 다중비교 보정 계획

### 8. Draft Plan Mandatory

> **원고 작성 전 반드시 `draft_plan.md`를 작성하고 사용자 승인 후 진행**

- **NEVER start drafting sections without first creating `draft_plan.md`**
- 분석 결과(`results/`) 확인 후 작성
- draft_plan.md가 없으면 섹션 작성을 거부
- 위치: Single = `drafts/draft_plan.md`, Multi = `drafts/paper{N}/draft_plan.md`

**필수 포함 내용 (1-8 필수, 9 선택):**
1. **Key message** — 핵심 메시지 (1-2문장)
2. **Tone & voice** — 논조/어조 (예: conservative & evidence-based, novel technique 강조, 기존 방법과 동등성 주장)
3. **Essential references** — 필수 인용 + 인용 목적 (배경/방법론/비교 대상)
4. **Evidence gap** — 추가 필요 근거 (검색 키워드 또는 논문 유형)
5. **Table/Figure plan** — 개수, 내용, Table vs Figure 결정
6. **Introduction outline** — Background → Gap → Purpose 흐름
7. **Discussion outline** — 주요 논점 3-5개, 비교할 선행연구
8. **Limitation points** — 예상 한계점 및 대응 논리
9. **Target word count** (선택) — 저널 기준 섹션별 목표 분량

### 9. Model Selection by Phase

> Plan 단계는 high-quality 모델, 작성 단계는 mid-quality 가능

| Phase | 권장 | 이유 |
|-------|------|------|
| 1 Setup | Opus/Sonnet | 검색·정리 |
| **2 Analysis** | **Opus** | analysis_plan 설계 |
| **3 Draft Plan** | **Opus** | 방향·논조·구성 결정 |
| 4 Draft | Sonnet (Opus 가능) | plan + evidence 기반 작성 |
| 5 Style Polish | Sonnet (Opus 가능) | 규칙 기반 |
| 6 QC | Sonnet (Opus 가능) | 체크리스트 기반 |
| 7 Finalize | Sonnet | DOCX 변환·서식 |
| **8 Revision** | **Opus** | 리뷰어 대응은 전략적 판단 |

**핵심 원칙:** Plan은 Opus로 잘 잡고 → 작성은 Sonnet으로도 OK. Phase 3 Draft Plan은 Plan Mode(`/plan`) 활용 권장.

---

## Natural Academic Writing Style

> **상세 가이드: `docs/writing_guide.md`** (CLAUDE.md는 워크플로만 담당, 중복 방지)

**Phase 5에서 적용할 writing_guide.md 섹션:**

| 영역 | writing_guide.md 섹션 | 주요 내용 |
|------|----------------------|-----------|
| 전역 규칙 | General Principles | 시제, Bold 금지, 약어 1회 정의, 임상 결과 주어, 동의어 혼용 금지, 숫자 서식, 문두 숫자 |
| 스타일 표 | Style Reference Tables | Voice & Tense / Transition / Verb Upgrades / Common Corrections / Statistical Notation / Hedging |
| 작문 원칙 | Writing Principles (4 Pillars) | Clarity / Conciseness / Objectivity / Consistency |
| 섹션별 규칙 | 01. Title ~ 10. Tables | 각 섹션 구조·구체 규칙·예시 |

**Phase 5 워크플로:** Style Reference Tables 읽기 → 섹션별 적용 → 4 Pillars 검토 → Dr. Editor polish

---

## Recommended Workflow

```
Phase 1: Setup
├── Define topic, journal, study design (this file)
├── Search references: /search-evidence [query] 또는 scripts/search_pubmed.py
├── Import by DOI: /import-doi [doi]
├── Save PDFs to knowledge/pdf/, register in knowledge/evidence.md
├── 핵심 논문은 knowledge/summaries/에 상세 요약
└── Read docs/writing_guide.md for target sections

Phase 2: Statistical Analysis — Opus 권장
├── Read docs/statistical_analysis_guide.md
├── Place raw data in data/
├── Create data/analysis_plan.md (필수, 사용자 승인 후 진행)
│   └── endpoint hierarchy, 검정법, 다중비교 보정, 결측 처리
├── Generate Python scripts in data/py/ (01_descriptive, 02_comparative, 03_regression)
├── Run analysis → results/ (table*_*.csv)
├── Generate drafts/table_*.md from results CSV
└── Generate figures → drafts/figures/

Phase 3: Draft Plan — Opus 권장
├── Create drafts/draft_plan.md (Rule 8: 1-8 필수, 9 선택)
├── 사용자 확인 후 Phase 4 진행
└── Multi-paper: drafts/paper{N}/draft_plan.md

Phase 4: Draft (in this order)
├── 04_methods.md      → framework        (Dr. Researcher B)
├── 05_results.md      → narrative        (Dr. Researcher B; refer to drafts/table_*.md)
├── 03_introduction.md → background & gap (Dr. Researcher A)
├── 06_discussion.md   → interpretation   (Dr. Researcher A; check vs intro)
├── 07_conclusion.md   → brief takeaway
├── 02_abstract.md     → summary (write LAST)
└── 01_title.md        → finalize

Phase 5: Style Polish
├── Apply writing_guide.md Style Reference Tables (Transition / Verb / Voice / Notation / Hedging)
├── Apply Writing Principles (4 Pillars: Clarity / Conciseness / Objectivity / Consistency)
└── Dr. Editor 최종 polish

Phase 6: QC (3 rounds critical, 6 recommended)
├── Round 1: Number consistency
├── Round 2: Reference verification (vs evidence.md)
├── Round 3: Logic & flow (Dr. Editor)
├── Round 4: Terminology/abbreviation/tense (권장)
├── Round 5: Statistical quality (Dr. Statistician, 권장)
├── Round 6: Critical review (overclaiming/bias/일반화 범위, 권장)
├── Document in review/qc_log.md
└── Run study-specific checklist (checklist_guide.md)

Phase 7: Finalize
├── Read docs/docx_guide.md
├── Compile to DOCX (title page / manuscript / 각 table 별도)
├── 파일명에 버전 표기 (기본 _YYMMDD)
├── Co-author review
└── Final read-through

Phase 8: Revision (리뷰어 코멘트 수신 후) — Opus 권장
├── Read docs/revision_guide.md
├── 리뷰어 코멘트 저장: review/reviewer_comments_REV{N}.md
├── drafts/revision/REV{N}/ 생성, 수정된 섹션만 _REV{N} 저장
├── Response letter → drafts/revision/REV{N}/response_letter_REV{N}.md
├── QC re-run (최소 Round 1-2)
└── output/revision/REV{N}/에 최종 DOCX + response letter
```

### Phase Completion Criteria

| Phase | Move to Next When |
|-------|-------------------|
| 1 → 2 | knowledge/evidence.md ≥10 verified refs, topic defined, data ready |
| 2 → 3 | analysis_plan.md approved, all analyses complete, tables generated |
| 3 → 4 | draft_plan.md approved (Rule 8 항목 1-8 완결, 9 선택) |
| 4 → 5 | All sections drafted, numbers match tables |
| 5 → 6 | Writing style rules applied, Dr. Editor reviewed |
| 6 → 7 | Min 3 QC rounds passed (6 recommended), checklist complete |
| 7 → Submit | Co-author approved, journal requirements met, versioned files in output/ |
| Submit → 8 | Reviewer comments received |
| 8 → Resubmit | Revised manuscript + response letter complete, QC re-run passed |

---

## Quick Commands

| Phase | Command | Action |
|-------|---------|--------|
| 1 Setup | `Setup project for [topic]` | Initialize folder structure |
| 1 Setup | `/search-evidence [query]` | PubMed 검색 → 선택 → evidence.md 등록 |
| 1 Setup | `/import-doi [doi]` | DOI로 가져와 evidence.md 등록 |
| 1 Setup | `Process new PDFs` | knowledge/pdf/ 스캔, 미등록 PDF를 evidence.md에 등록 |
| 1 Setup | `Read writing guide for [section]` | Load section-specific guidance |
| 2 Analysis | `Analyze data` | analysis_plan.md 작성 (필수, 승인 후) |
| 2 Analysis | `Generate analysis scripts` | data/py/ 스크립트 생성 |
| 2 Analysis | `Run analysis` | 실행 → results/ |
| 2 Analysis | `Generate tables` | results CSV → drafts/table_*.md |
| 2 Analysis | `Generate figures` | Read figure_guide.md → drafts/figures/ |
| 2 Analysis | `Check figure quality` | DPI, 색맹 팔레트, 흑백 구분 확인 |
| 2 Analysis | `Summarize statistics` | 통계 결과 전체 개요 |
| 3 Plan | `Create draft plan` | drafts/draft_plan.md (Opus 권장) |
| 3 Plan | `Review draft plan` | 검토 및 수정 제안 |
| 4 Draft | `Draft [section]` | 섹션 작성 (draft_plan.md 기반) |
| 4 Draft | `Draft [section] as Dr. [Expert]` | 특정 expert 관점으로 작성 |
| 4 Draft | `Review as Dr. [Expert]` | Expert 피드백 |
| 4 Draft | `Team review [section]` | 모든 expert 리뷰 |
| 5 Polish | `Apply writing style to [section]` | Natural Academic Writing rules |
| 5 Polish | `Check transitions` | Weak transitions (but, however 남용) 찾기 |
| 5 Polish | `Upgrade verbs in [section]` | 학술 동사로 교체 |
| 5 Polish | `Polish as Dr. Editor` | 최종 언어 다듬기 |
| 6 QC | `Run QC round [1-6]` | qc_guide.md 따라 실행 |
| 6 QC | `Check number consistency` | 섹션 간 숫자 일치 |
| 6 QC | `Verify references` | evidence.md 대조 |
| 6 QC | `Check logic flow` | 흐름 일관성 |
| 6 QC | `Run checklist for [study type]` | STROBE/CONSORT/PRISMA/CARE |
| 7 Finalize | `Compile manuscript` | docs/docx_guide.md 따라 DOCX 변환 |
| 7 Finalize | `Format references for [journal]` | 저널 인용 스타일 적용 |
| 7 Finalize | `Generate submission checklist` | 제출 전 검증 |
| 8 Revision | `Analyze reviewer comments` | Major/Minor 분류 + 대응 전략 |
| 8 Revision | `Draft response to reviewer [N]` | 특정 리뷰어 응답서 |
| 8 Revision | `Draft response letter` | 전체 응답서 |
| 8 Revision | `Review response letter` | Dr. Editor 검토 |
| 8 Revision | `Check response completeness` | 응답서 ↔ 원고 수정 일치 |

---

## Notes

- 상세 가이드는 `docs/` 폴더에 — 필요할 때 읽어서 context 절약
- AI-generated citation은 항상 실제 논문과 대조 검증
- 제출 전 최소 3 QC rounds + 인간 expert 리뷰 필수

**Expert simulation** (drafting 시 `docs/expert_roles.md` 참조):
- **Dr. Researcher A** — Clinical (Introduction, Discussion)
- **Dr. Researcher B** — Methodology (Methods, Results, Tables)
- **Dr. Statistician** — Statistical validation
- **Dr. Editor** — Final polish, consistency
