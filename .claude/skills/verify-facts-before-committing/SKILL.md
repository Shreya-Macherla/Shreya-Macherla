---
name: verify-facts-before-committing
description: Load before editing README.md in this repo (the GitHub profile page) — specifically the Publications section (DOIs, years, journal names), the header badges (LinkedIn/Email/ORCID links), or the Selected Projects table (links to other repos). Trigger on any edit that adds/changes a link, date, citation, or accuracy claim.
---

# Verify facts before committing to this README

This repo is a single README.md rendered on the GitHub profile page — there's no code to test,
but git history shows a repeated pattern of shipping a factual error and then needing a follow-up
fix commit:

- `e7d3d17` "Fix PLOS ONE link to correct DOI 0318647" — a wrong DOI was committed and lived in
  the public profile until caught.
- `0749fa0` "Fix publication year to 2025" — a wrong year was committed.
- `531749e` "Restore ORCID badge in header, remove PLOS ONE from headline" and `b53a5dd` "Move
  Google Scholar and ORCID to Publications section" — badge/link placement churned across
  multiple commits, implying it wasn't checked against a clear plan before landing.
- `f8dd948` "Mark third publication as forthcoming 2027" — publication status (published vs.
  in-press vs. forthcoming) needed a correction after the fact.

None of these are typos — they're factual claims (a DOI, a publication year, a review status)
that were wrong when committed and stayed wrong until someone caught it in a separate commit.
For a profile page whose entire purpose is to represent real credentials accurately, that's the
one failure mode worth guarding against.

## Before committing any change to README.md

1. **Every DOI/link gets clicked, not assumed.** If you add or touch a link under
   `### Publications` or a project link under `### Selected Projects`, open it (or fetch it) and
   confirm it resolves to the thing the surrounding text claims it is — not just that it's
   well-formed markdown.
2. **Every date/year claim is cross-checked against its source**, not carried over from memory or
   a previous draft. "Forthcoming 2027" vs "2025" vs "2024" are different claims — check which one
   is currently true before writing it.
3. **Badge/section moves are one commit, not several.** The ORCID/Scholar badge history shows
   three separate commits moving the same two badges around. If you're restructuring header vs.
   Publications section placement, decide the final layout first, then make one commit — don't
   ship an intermediate state.
4. **Grep for the DOI/ORCID/link before and after editing** to eyeball-diff what actually changed:
   ```bash
   git diff README.md | grep -E "doi|orcid|scholar|springer|journals.plos"
   ```

## When NOT to use this skill

This repo has no other code, tests, or CI — there is nothing else here to check. If you're
editing one of the *linked* project repos (e.g. CryptoCurrency-Carbon-Emission,
Human-Activity-Recognition-using-CNN-and-RNN), use that repo's own `.claude/skills/`, not this
one — this skill only covers the profile README's own factual claims, not the accuracy of the
project repos it links to.
