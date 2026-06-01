# Citation Audits Against AI Hallucinations

## Verifying References by Identity, Not Search Hits

This repository contains a Codex skill for auditing bibliographies that may contain AI-hallucinated, distorted, duplicated, or weakly sourced references.

The core idea is simple:

> A reference is verified only when evidence confirms the same work, not merely a similar search result.

## Why This Exists

AI-generated reference lists often contain three different failure modes:

- fabricated works that do not exist;
- real works with distorted authors, years, venues, or page ranges;
- real but hard-to-search works that are wrongly deleted after a shallow search.

The last failure mode is especially dangerous. A failed first search should produce `pending verification`, not `fabricated`.

## Method

The skill uses a conservative identity-based standard:

- `◎ 真实`: strong source or multi-source confirmation of the same work.
- `○ 基本可信`: credible evidence supports the same work, but stronger first-party/database evidence is still desirable.
- `△ 待核`: not confirmed yet; do not delete.
- `重复`: duplicate citation.
- `疑似编造/不采用`: only when there is explicit conflict or repeated multi-source failure plus abnormal metadata.

Preferred evidence includes CNKI, Wanfang, VIP, journal/publisher pages, DOI/Crossref, university repositories, Google Scholar, Baidu Scholar, author-unit pages, and exact reference-list citations.

## Repository Layout

```text
citation-audit-against-ai-hallucinations/
  README.md
  citation-audit-against-ai-hallucinations/
    SKILL.md
    agents/openai.yaml
    references/audit-standard.md
    scripts/build_audit_workbook.py
```

## Use As A Codex Skill

Copy the skill folder into your Codex skills directory:

```bash
cp -r citation-audit-against-ai-hallucinations ~/.codex/skills/
```

Then ask Codex to use `citation-audit-against-ai-hallucinations` to audit a bibliography or generate an evidence workbook.

## Build An Audit Workbook

The included script formats audited CSV rows into a review workbook:

```bash
python citation-audit-against-ai-hallucinations/scripts/build_audit_workbook.py input.csv output.xlsx
```

Minimal CSV:

```csv
编号,原参考文献,最终判定,主证据URL,多源证据URL,截图文件,可靠性说明,问题备注
1,Example reference,◎ 真实,https://example.org,https://example.edu,ref-001.png,Official record matches title author year,
```

## Practical Rule

Use this line when explaining the audit to collaborators:

> We are preventing AI-fabricated references, but we are also preventing false deletion of real references. Missing search results stay pending unless there is clear contradictory evidence.

## License

MIT
