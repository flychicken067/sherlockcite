# Identity-Based Citation Audit Standard

Use this checklist when auditing references that may have been generated, distorted, or fabricated by AI.

## Decision Logic

Ask these questions in order:

1. Does any source show the exact or near-exact title?
2. Do the author names match, at least first author or responsible organization?
3. Does the publication year match?
4. Does the source venue match: journal, publisher, conference, institution, or report issuer?
5. Do volume, issue, pages, DOI, or degree institution match?
6. Is the evidence an authoritative record, a scholarly index, an institutional page, or only a secondary mention?

## Status Definitions

### VERIFIED

Use when identity is confirmed by:

- one strong source with enough metadata, or
- two independent weaker sources that converge on the same title, author, source, and year.

Examples:

- CNKI, Wanfang, or VIP record lists title, author, journal, and year.
- Journal table of contents lists title, author, volume, issue, and pages.
- DOI, Crossref, or publisher page lists title, author, journal, and year.
- University repository lists dissertation title, author, institution, and year.

### PLAUSIBLE

Use when the work appears real, but the evidence is not yet a complete first-party or database record.

Examples:

- Baidu Scholar or Google Scholar shows title, author, year, and source.
- Author-unit page lists the work as a publication.
- Another paper's reference list cites the same work exactly.
- A journal directory mirror shows the title and source but not all fields.

### PENDING

Use when evidence is incomplete or unavailable.

Examples:

- Only similar titles were found.
- Search result count is zero.
- The page is blocked by login or security verification.
- Title matches but author/source/year is missing.
- A new or obscure work is likely real but not publicly indexed.

Do not recommend deletion from this status.

### DUPLICATE

Use when the same work appears more than once. Keep original numbering and identify the earlier duplicate when possible.

### SUSPECT

Use only with explicit reasons:

- the cited issue/year does not exist;
- the title is attributed to a different author/source/year in authoritative records;
- multiple independent searches find no record and the citation contains structural anomalies;
- the cited DOI resolves to a different work.

## Source Strategy

Chinese journal article:

1. CNKI public record
2. Wanfang
3. VIP
4. journal website or table of contents
5. Baidu Scholar
6. author-unit or institution page
7. exact reference-list citation in another paper

Chinese dissertation:

1. CNKI doctoral/master database
2. Wanfang dissertations
3. university repository or library
4. Baidu Scholar
5. advisor/student CV or institution page

English paper:

1. DOI resolver or Crossref
2. publisher page
3. Google Scholar
4. Semantic Scholar or OpenAlex
5. arXiv, RePEc, or SSRN when domain-appropriate

Reports and policies:

1. official issuer site
2. government or association archive
3. publisher or institutional repository
4. library catalog or stable secondary record

## Anti-Patterns

- Do not verify by Tavily/search rank alone.
- Do not accept an author profile as article evidence unless it lists the specific work.
- Do not accept a same-topic paper as identity evidence.
- Do not downgrade to fabricated because a database requires login.
- Do not mix up evidence URL and searched title; the screenshot must show the cited work.

## Human-Facing Explanation

Use this short explanation in deliverables:

> This audit verifies whether each reference points to the same real work, not whether a search engine returns a similar result. A missing public search result is treated as pending verification, not fabrication. Only explicit metadata conflicts or repeated failure across independent source paths justify a suspect classification.
