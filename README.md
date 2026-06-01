# SherlockCite

## Verifying References by Identity, Not Search Hits

SherlockCite is a Codex skill for auditing bibliographies that may contain AI-hallucinated, distorted, duplicated, or weakly sourced references.

The core idea:

> A citation is verified only when evidence confirms the same work, not merely a similar search result.

## Why This Exists

AI-generated reference lists often contain three different failure modes:

- fabricated works that do not exist;
- real works with distorted authors, years, venues, or page ranges;
- real but hard-to-search works that are wrongly deleted after a shallow search.

The last failure mode is especially dangerous. A failed first search should produce `pending verification`, not `fabricated`.

## Method

SherlockCite uses a conservative identity-based standard:

- `VERIFIED`: strong source or multi-source confirmation of the same work.
- `PLAUSIBLE`: credible evidence supports the same work, but stronger first-party or database evidence is still desirable.
- `PENDING`: not confirmed yet; do not delete.
- `DUPLICATE`: duplicate citation.
- `SUSPECT`: use only when there is explicit conflict or repeated multi-source failure plus abnormal metadata.

Preferred evidence includes CNKI, Wanfang, VIP, journal or publisher pages, DOI/Crossref, university repositories, Google Scholar, Baidu Scholar, author-unit pages, and exact reference-list citations.

## Repository Layout

```text
sherlockcite/
  README.md
  sherlockcite/
    SKILL.md
    agents/openai.yaml
    references/audit-standard.md
    scripts/build_audit_workbook.py
  examples/
    sample-audit.csv
```

## Use As A Codex Skill

Copy the skill folder into your Codex skills directory:

```bash
cp -r sherlockcite ~/.codex/skills/
```

Then ask Codex to use `sherlockcite` to audit a bibliography or generate an evidence workbook.

## Build An Audit Workbook

The included script formats audited CSV rows into a review workbook:

```bash
python sherlockcite/scripts/build_audit_workbook.py input.csv output.xlsx
```

Minimal CSV:

```csv
id,reference,status,source_url,secondary_urls,screenshot_file,evidence_note,problem_note
1,Example reference,VERIFIED,https://example.org,https://example.edu,ref-001.png,Official record matches title author year,
```

## Practical Rule

Use this line when explaining the audit to collaborators:

> We are preventing AI-fabricated references, but we are also preventing false deletion of real references. Missing search results stay pending unless there is clear contradictory evidence.

## License

MIT
