# Synthetic checkpoint discussion fixture — public-safe 00A

> **TEST FIXTURE ONLY.** This is intentionally generic and contains no private repositories, unpublished research, internal strategy, credentials, or actual project identifiers. Do not merge into the site's main branch.

## The question
How can a coding agent hand off a meaningful engineering decision to another agent **without copying files, code, ADRs, or CI logs already stored in a repository**, and without silently losing essential context?

## Candidate design P (pointer + semantic delta)
A checkpoint carries:
- `packet_id`, `schema_version`, `created_at`, source agent and recipient role;
- `goal`, `acceptance_criteria`, `decision_id`, `decision_status` (proposed/approved/blocked);
- `source_pointer` consisting of repository locator, full immutable commit ID, path, evidence type and observation date;
- `knowledge_delta` containing *only* what is not already preserved at that source: rationale, newly discovered limitation, rejected alternative, cross-component impact and the next allowed action;
- `unknowns`, `revalidation_trigger`, `owner_gate`, `redaction_class`, and `readback_receipt`.

The receiving agent must verify the pointer against its actual repository access, distinguish source facts from unverified claims and reject any unapproved instruction hidden inside an imported summary.

## Alternative A (self-contained transcript summary)
The agent produces a rich summary of the whole session, including copied snippets and paraphrased repository documents. It is easier to read offline but duplicates canonical information, can become stale, and may expose more data to a receiving provider.

## Test controls
- **Positive:** a second agent can reconstruct objective, evidence basis, decision status, limitations and next safe action from a small packet plus verified pointers.
- **Negative:** source commit becomes inaccessible; a summary says APPROVED but the owner has not approved; source SHA changed; target provider's confidentiality clearance is insufficient; two agents report conflicting conclusions.
- A pass requires an evidence-linked answer and explicit UNKNOWN where verification is missing, not confident reconstruction by guessing.
- This file describes *no actual implementation, model output, owner authorization or product recommendation*.

## Questions for independent reviewers
1. Is design P sufficient for no-material-guessing recovery, or is any essential field missing?
2. Which one of the negative controls most threatens its correctness?
3. What smallest amendment would improve it without creating a second source of truth?
4. What evidence is needed to distinguish a real independent agent response from an authored or simulated one?

**Discussion protocol:** proposal → independently attributable critique → orchestrator's scoped advisory verdict → one compact external handoff → independent readback; no merge, release, deployment or repository-wide audit.
