---
name: sherlockcite
description: Verify bibliographies and reference lists for possible AI hallucinations using identity-based, multi-source evidence. Use when Codex needs to audit references from Word/Excel/CSV/PDF files, distinguish real citations from fabricated or malformed ones, compare CNKI/Wanfang/VIP/Google Scholar/Baidu Scholar/publisher evidence, create evidence screenshots, or produce an Excel action list that avoids deleting real references merely because a first search fails.
---

# SherlockCite

Audit references by identity, not by search-result luck. A citation is credible only when evidence points to the same work: title, author, source, year, and ideally volume/issue/pages, DOI, publisher, or degree-granting institution.

## Core Standard

Use these statuses consistently:

- `VERIFIED`: a strong authoritative source confirms the same work, or two independent sources cross-confirm it.
- `PLAUSIBLE`: credible but weaker evidence confirms the same work, such as Baidu Scholar, Google Scholar, an author-unit page, a journal table-of-contents mirror, or another paper's exact reference list.
- `PENDING`: evidence is incomplete, unstable, similar-but-not-identical, or blocked. Do not recommend deletion.
- `DUPLICATE`: the same work appears more than once. Mark the earlier duplicate when known.
- `SUSPECT`: use only when multiple source paths fail and there is a clear conflict, such as nonexistent issue/year, incompatible author/title/source, impossible publication metadata, or a DOI that resolves to a different work.

Never treat "first search failed", "Tavily top result is wrong", "Google Scholar is blocked", or "CNKI public page is unavailable" as proof of fabrication.

## Workflow

1. Extract references with stable original numbering.
2. Normalize candidate identity fields:
   - title
   - authors
   - year
   - journal, book, conference, publisher, report issuer, or degree institution
   - volume, issue, pages, DOI, accession number, or repository identifier when available
3. Search by source type:
   - Chinese articles: CNKI public record, Wanfang, VIP, journal website, Baidu Scholar, institutional pages, cited-by/reference-list evidence.
   - Chinese dissertations: CNKI degree database, Wanfang dissertations, university repository/library, Baidu Scholar.
   - English works: DOI/Crossref, publisher page, Google Scholar, Semantic Scholar, OpenAlex, arXiv/RePEc/SSRN when appropriate.
   - Reports/policies: publisher, government, association, archive copy, or institutional announcement.
4. Capture evidence only from pages that show enough identity fields. If screenshots are blocked, record the URL and a screenshot-failure reason.
5. Record both positive evidence and failed paths for `PENDING` and `SUSPECT`.
6. Produce an Excel workbook with a reviewer-facing summary, detailed audit rows, and next-step actions.

## Evidence Rules

Prefer sources in this order:

1. Database or official record: CNKI, Wanfang, VIP, Crossref/DOI, publisher/journal page, university repository.
2. Institutional record: author unit, research center, government/association page.
3. Scholarly index: Google Scholar, Baidu Scholar, Semantic Scholar, OpenAlex, RePEc.
4. Secondary citation: another paper's reference list that exactly cites the same work.

For `VERIFIED`, require title match plus at least two of author/source/year/volume-page, unless one strong source directly lists the full metadata.

For `PLAUSIBLE`, require title match plus at least one of author/source/year. Do not upgrade to `VERIFIED` from a single scholarly-search result alone.

For `PENDING`, write what was tried and what is missing. Use conservative language: "not confirmed yet", not "fake".

## Common Failure Modes

- Search engines return a thematically similar paper with a different title or year.
- The top result is an author profile, index page, or unrelated PDF.
- Chinese scholarly search may work better with exact title only than with `author + title`.
- Baidu Scholar may have different old/new entry URLs; prefer visible-browser verification when automation returns security checks or empty results.
- Newly published or niche Chinese journals may be present in journal websites or Wanfang/VIP before public CNKI pages are easy to find.

## Output Columns

Use these columns for detailed sheets:

`id`, `reference`, `status`, `normalized_title`, `source_url`, `secondary_urls`, `screenshot_file`, `matched_fields`, `evidence_note`, `problem_note`, `next_action`

Recommended sheets:

- `Summary`: concise distribution and reviewer guidance.
- `Audit Details`: one row per original reference.
- `Action List`: `PENDING`, `DUPLICATE`, weak `PLAUSIBLE`, and `SUSPECT` rows sorted by priority.
- `Evidence Log`: evidence files, URLs, screenshot status.
- `Method Notes`: explain that the audit verifies identity, not search-hit similarity.

## Script

Use `scripts/build_audit_workbook.py` when you have a CSV of audited rows and need a polished Excel workbook. It accepts flexible English and common Chinese column names, then writes English output sheets.

Minimal input columns:

```csv
id,reference,status,source_url,secondary_urls,screenshot_file,evidence_note,problem_note
1,Example reference,VERIFIED,https://...,https://...,ref-001.png,CNKI record matches title/author/year,
```

Run:

```bash
python scripts/build_audit_workbook.py input.csv output.xlsx
```

Read `references/audit-standard.md` when you need a fuller checklist or need to explain the method to a human reviewer.
