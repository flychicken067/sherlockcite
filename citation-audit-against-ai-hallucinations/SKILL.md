---
name: citation-audit-against-ai-hallucinations
description: Verify bibliographies and reference lists for possible AI hallucinations using identity-based, multi-source evidence. Use when Codex needs to audit references from Word/Excel/CSV/PDF files, distinguish real citations from fabricated or malformed ones, compare CNKI/Wanfang/VIP/Google Scholar/Baidu Scholar/publisher evidence, create evidence screenshots, or produce an Excel action list that avoids deleting real references merely because a first search fails.
---

# Citation Audits Against AI Hallucinations

Audit references by identity, not by search-result luck. A reference is credible only when the evidence points to the same work: title, author, source, year, and ideally volume/issue/pages or degree-granting institution.

## Core Standard

Use these statuses consistently:

- `◎ 真实`: a strong authoritative source confirms the same work, or two independent sources cross-confirm it.
- `○ 基本可信`: credible but weaker evidence confirms the same work, such as Baidu Scholar/Google Scholar, an author-unit page, a journal table of contents mirror, or another paper's exact reference list.
- `△ 待核`: evidence is incomplete, unstable, similar-but-not-identical, or blocked. Do not recommend deletion.
- `重复`: the same work appears more than once. Mark the duplicate target if known.
- `疑似编造/不采用`: use only when multiple source paths fail and there is a clear conflict, such as nonexistent issue/year, incompatible author/title/source, or impossible publication metadata.

Never treat "first search failed", "Tavily top result is wrong", or "CNKI public page is unavailable" as proof of fabrication.

## Workflow

1. Extract references with stable original numbering.
2. Normalize the candidate identity fields:
   - title
   - authors
   - year
   - journal/book/conference/degree institution
   - volume/issue/pages, DOI, publisher, or database accession when available
3. Search in this order unless the source type suggests otherwise:
   - Chinese articles: CNKI public record, Wanfang, VIP, journal website, Baidu Scholar, institutional pages, cited-by/reference-list evidence.
   - Chinese dissertations: CNKI degree database, Wanfang dissertations, university repository/library, Baidu Scholar.
   - English works: DOI/Crossref, publisher page, Google Scholar, Semantic Scholar, OpenAlex, arXiv/RePEc/SSRN when appropriate.
   - Reports/policies: publisher/government/association website, archive copy, institutional announcement.
4. Capture evidence only from pages that show enough identity fields. If screenshots are blocked, record the URL and a screenshot-failure reason.
5. Record both positive evidence and failed paths for `△ 待核` and `疑似编造/不采用`.
6. Produce an Excel workbook with a friend/client-facing summary, detailed audit rows, and next-step actions.

## Evidence Rules

Prefer sources in this order:

1. Database or official record: CNKI, Wanfang, VIP, Crossref/DOI, publisher/journal page, university repository.
2. Institutional record: author unit, research center, government/association page.
3. Scholarly index: Google Scholar, Baidu Scholar, Semantic Scholar, OpenAlex, RePEc.
4. Secondary citation: another paper's reference list that exactly cites the same work.

For `◎ 真实`, require title match plus at least two of author/source/year/volume-page, unless one strong source directly lists the full metadata.

For `○ 基本可信`, require title match plus at least one of author/source/year. Do not upgrade to `◎` from a single scholarly-search result alone.

For `△ 待核`, write what was tried and what is missing. Use conservative language: "not confirmed yet", not "fake".

## Common Failure Modes

- Search engines return a thematically similar paper with a different title or year.
- The top result is an author profile, index page, or unrelated PDF.
- Chinese scholarly search works better with exact title only than with `author + title`.
- Baidu Scholar may have different old/new entry URLs; prefer visible-browser verification when automation returns security checks or empty results.
- Newly published or niche Chinese journals may be present in journal websites or Wanfang/VIP before public CNKI pages are easy to find.

## Output Columns

Use these columns for detailed sheets:

`编号`, `原参考文献`, `最终判定`, `规范化题名`, `主证据URL`, `多源证据URL`, `截图文件`, `匹配字段`, `可靠性说明`, `问题备注`, `建议动作`

Recommended sheets:

- `给朋友看的结论`: concise summary and distribution.
- `最终核验明细`: one row per original reference.
- `待处理清单`: `△ 待核`, duplicates, and weak `○` rows sorted by priority.
- `来源与截图记录`: evidence files, URLs, screenshot status.
- `方法说明`: explain that the audit verifies identity, not search-hit similarity.

## Script

Use `scripts/build_audit_workbook.py` when you have a CSV of audited rows and need a polished Excel workbook. It accepts flexible column names and adds summary/action sheets.

Minimal input columns:

```csv
编号,原参考文献,最终判定,主证据URL,多源证据URL,截图文件,可靠性说明,问题备注
1,Example reference,◎ 真实,https://...,https://...,ref-001.png,CNKI record matches title/author/year,
```

Run:

```bash
python scripts/build_audit_workbook.py input.csv output.xlsx
```

Read `references/audit-standard.md` when you need a fuller checklist or need to explain the method to a human reviewer.
