# Independent Compliance Audit — AI Pre-Scan

**Auditor:** Vittal
**System builder:** Ugo (Nnanyelugo Ahukannah)
**System audited:** AI Pre-Scan — `github.com/nyelugo/PROJECT-AI-Pre-Scan`
**Audit date:** 2026-08-19
**Materials reviewed:** System brief materials only (README, proposal, architecture, elevator pitch, report spec, LESSONS.md) — no self-audit, risk classification, gap analysis, or compliance memo produced by the builder was consulted. See [01-clarifying-questions-log_EU AI Act.md](01-clarifying-questions-log_EU%20AI%20Act.md) for the questions this report's uncertainties depend on.

---

## Section 1 — System Summary

AI Pre-Scan is a research agent, built primarily on LangGraph, that takes a company name as its only input and produces an evidence-backed first-draft inventory of the AI systems that company appears to use — sourced entirely from public information (websites, careers pages, vendor changelogs, press coverage, company registries). Each finding in the output carries a quoted passage, a source, and a three-valued confidence rating (Evidenced / Inferred / Undetermined); nothing is asserted without a traceable quote, and thin evidence is reported as "undetermined" rather than guessed. Alongside the inventory, the system generates a client-specific discussion list — the questions a human adviser still needs to ask, phrased for a non-expert. The intended buyer is an external compliance adviser (the brief's persona "Maria," running a small consultancy with ~40 SME clients) who feeds each identified system into a separate, deterministic third-party tool (the Future of Life Institute's EU AI Act Compliance Checker) to get the actual legal classification. The system is explicit and structural about *not* performing risk classification, citing obligations, or making legal determinations itself — that boundary is described as the core design decision, not a disclaimer.

---

## Section 2 — Risk Classification

### Classification table

| Question | Answer |
|---|---|
| Does this system fall under any prohibited category (Article 5)? | No. It performs public-source research and evidence extraction about organisations; it does not perform social scoring, biometric categorisation/identification, subliminal manipulation, exploitation of vulnerabilities, or predictive-policing-style individual risk assessment. |
| Does this system operate in any of the eight Annex III areas? | No, on the system's own function. AI Pre-Scan's subject is *organisations*, not natural persons, and its output is explicitly schema-restricted to functional description with "no regime vocabulary" (`docs/report-spec.md`, hard rule 1). It does not itself make or assist decisions about employment, credit, law enforcement, migration, education, essential services, biometric ID, or justice/democratic processes for any natural person — even though the *systems it reports on* may operate in those domains for its clients' clients. |
| If Annex III: does it significantly influence decisions in that area, or is it narrow/preparatory? | N/A under the above — but worth stating precisely: even if one argued AI Pre-Scan operates "adjacent to" employment-domain compliance work, its own output is restricted by design to fact-finding and evidence with a mandatory downstream deterministic legal checker (Article 6(3)-style narrow/preparatory character: it performs a preparatory task ahead of a decision, and the decision is made elsewhere, by a human adviser and a separate rules engine). |
| Does this system interact with end users or generate content requiring disclosure (Article 50)? | **Yes, likely.** GPT-4o is used to generate the report's findings and summary text, and that generated text is handed to a third party (the client) by the adviser, per the README ("She hands them to clients"). This is the live classification question — see below. |

### First-pass risk tier: **Limited risk / transparency obligation (Article 50), not high-risk**

**One-sentence justification:** AI Pre-Scan does not fall under Article 5 or any Annex III high-risk category because its function and subject matter are restricted by design to organisational fact-finding with a mandatory hand-off to a separate deterministic legal tool, but it likely triggers the Article 50(2) obligation to mark AI-generated content as such, because it generates report text via GPT-4o that reaches a third party (the client) without a documented human-editorial step that would qualify for the Article 50(4) exemption.

**Areas of uncertainty:**
- Whether the adviser's review before handing over a report constitutes "editorial control and human review" sufficient for the Article 50(4) exemption, or whether the report is close enough to a direct GPT-4o output that Article 50(2) marking is required (see Q3 in the questions log).
- Whether, at real-client scale, incidental naming of individuals inside quoted evidence (LESSONS.md: "a quote may name someone") ever rises to a level where GDPR's profiling/special-category provisions are implicated — this doesn't change the AI Act tier, but it is a live parallel-law question (see Finding 3).

---

## Section 3 — Role Map

| Role | Party | Key obligations that flow from this role |
|---|---|---|
| **Provider** | Ugo (Nnanyelugo Ahukannah), as builder/author of AI Pre-Scan — pending confirmation of any future legal entity (Q5) | Article 50(2) synthetic-content marking (if applicable); Article 4 AI-literacy measures for staff/deployers; general product-safety and documentation duties even at limited-risk tier; GDPR controller/processor obligations for any personal data the system's pipeline touches |
| **Deployer** | The compliance adviser (persona "Maria") who runs scans against client company names and distributes reports | Article 50 downstream disclosure if she forwards AI-generated content without adequate human authorship; Article 4 AI-literacy for how she and her staff use and interpret "undetermined" outputs; professional/consumer-protection duties around what she represents the report as to her own clients |
| **Sub-processor / vendor — OpenAI** | Extraction (GPT-4o) and embeddings (`text-embedding-3-small`), US-based | GDPR: international-transfer mechanism required (Art. 44–49); the project's own docs confirm this is **not yet in place** |
| **Sub-processor / vendor — Pinecone** | Per-scan evidence vector store and (designed, unbuilt) vendor corpus, US-based | Same as above — transfer mechanism required, not yet in place |
| **Sub-processor / vendor — Serper, NewsAPI** | Web/news search | Ordinary data-processing agreements; lower sensitivity (search queries, not evidence storage) |
| **Sub-processor / vendor — GLEIF, Wikidata** | Company identity resolution | Keyless, public registries — minimal additional obligation |
| **Sub-processor / vendor — Notion (via n8n)** | Delivery/filing of the finished report | Processor agreement scope depends on report content (which may include the incidental personal-data quotes discussed above) |
| **Downstream tool — Future of Life Institute's Compliance Checker** | Not integrated as a data processor; the adviser manually carries facts into it | No direct data-sharing obligation identified; worth noting the brief is careful to describe this as feeding, not embedding, the third-party tool |
| **Data subjects (indirect)** | Any named individual appearing incidentally in quoted evidence passages | Not a "role" under the AI Act, but relevant to the GDPR finding below |

---

## Section 4 — Compliance Findings

### Finding 1 — Article 50(2) synthetic-content marking
**Severity:** Significant
**Description:** AI Pre-Scan generates report text via GPT-4o that an adviser hands directly to a client (README: "Reports print. She hands them to clients"). The report-spec fixes a standing legal-disclaimer notice but that notice addresses legal scope ("this is not legal advice"), not AI-generation provenance. Nothing in the reviewed documentation states that the report or its findings are marked as AI-generated content in the sense Article 50(2) requires, and the standing notice is explicitly "not editable by any generation step" (`docs/report-spec.md`, hard rule 7), which cuts against treating the adviser's pass-through as a substantive human-editorial step under Article 50(4).
**Recommended action:** Add an explicit, machine-readable "this report was generated using an AI system" disclosure distinct from the legal-scope notice, or document the specific human-review step that would qualify for the Article 50(4) exemption and make that step verifiable (not just assumed).
**Escalation needed?** Yes — to the builder, before any real client use, since this is a launch-blocking transparency gap rather than a design question.

### Finding 2 — GDPR international transfer mechanism absent (parallel legal issue)
**Severity:** Significant
**Description:** The system's own documentation states plainly that OpenAI and Pinecone process data outside the EEA and that "processor agreements and a transfer mechanism are not yet in place and are required before any real client use" (`docs/proposal.md` §9; confirmed again in `LESSONS.md`). This is not an AI Act finding but a directly relevant parallel compliance issue the lab asks to flag, and it is unusually well self-documented by the builder already.
**Recommended action:** Put Standard Contractual Clauses (or equivalent Article 46 mechanism) in place with OpenAI and Pinecone before any scan touches a real client's data, and document the legal basis (likely legitimate interest, given the incidental and minimised nature of the personal data) for the processing that does occur.
**Escalation needed?** Yes — to whoever will act as the legal/compliance reviewer for this project; this is squarely a "before any real client use" gate the builder has already named themselves.

### Finding 3 — Incidental personal data in quoted evidence, no documented legal-basis assessment
**Severity:** Minor
**Description:** `LESSONS.md` acknowledges "public sources still name people, so a quote may name someone," and frames the mitigation as minimisation (only validated passages stored, purged per scan) rather than a legal-basis determination. Minimisation is good practice and reduces exposure, but it is not itself a GDPR Article 6 legal basis, and the documentation doesn't show a legitimate-interest assessment or equivalent having been performed.
**Recommended action:** Document a short legitimate-interest assessment (or equivalent) covering the incidental appearance of named individuals in quoted source text, even though minimisation already limits the exposure.
**Escalation needed?** No — good practice to close before scaling, not a launch blocker given the minimisation already in place.

### Finding 4 — No high-risk or prohibited-practice exposure identified
**Severity:** N/A (positive finding, recorded for completeness)
**Description:** The system's structural refusal to classify risk, cite obligations, or make legal determinations (`docs/proposal.md` §3; `docs/report-spec.md` hard rule 1) is a genuine and well-enforced design boundary, not just stated intent — it is reflected in the schema (no regime vocabulary permitted), in the evidence gate, and in the evaluation metrics (honest-refusal rate measured directly). This materially reduces the system's own AI Act exposure by keeping it out of Annex III territory regardless of how advisers eventually use its output.
**Recommended action:** None — maintain this boundary as new features are added; it is the single biggest reason this system doesn't need a high-risk conformity assessment.
**Escalation needed?** No.

### Finding 5 — Evidence-currentness safeguard was inert in production until recently fixed (context for reliability, not an Act violation)
**Severity:** Minor
**Description:** `LESSONS.md` documents that the evidence gate's currentness check was effectively a no-op across every evaluation run (every fetched page was marked "current" purely because the fetch returned HTTP 200), meaning the safeguard cited in the architecture as the reason for choosing LangGraph over a simpler pipeline had never actually executed. This isn't an Act compliance failure — nothing here misclassifies risk or violates a prohibited practice — but it's relevant to Finding 1's editorial-review question: if the underlying evidence pipeline had an inert safeguard for a meaningful stretch, that strengthens the case for a genuine human-review step before reports reach clients, rather than treating adviser pass-through as sufficient.
**Recommended action:** Confirm (not just assert) the fix is deployed and covered by a regression test before the next live scan reaches a real client, which `tests/test_fetch_extract.py`'s `test_a_200_does_not_make_stale_content_current` appears to do.
**Escalation needed?** No — already caught and fixed by the builder; noted here only because it's directly relevant to Finding 1.

---

## Section 5 — Overall Recommendation

**Proceed with conditions.**

No blocking findings were identified — AI Pre-Scan does not fall into a prohibited category or an Annex III high-risk domain, and its core design (evidence gate, three-valued confidence, structural refusal to classify risk) is unusually well-aligned with what the Act would want from a system like this. However, two Significant findings must be closed before any real client's company name is scanned and any report is handed to an actual client: (1) resolve whether the report needs an explicit Article 50(2) AI-generation disclosure, given the "not editable by any generation step" standing-notice constraint, and (2) put a GDPR international-transfer mechanism in place for OpenAI and Pinecone — which the builder has already flagged as an open item in the project's own documentation. Neither requires a redesign; both are closeable gates.

---

## Section 6 — What This Report Is Not

This report is not a legal opinion, not a conformity assessment, and not a certification. It reflects an independent, time-boxed review of the system brief and public project documentation only, conducted without access to the builder's own self-audit, and it should not be relied on as a substitute for legal counsel. All conclusions here should be verified with qualified legal counsel before AI Pre-Scan (or any report it produces) is placed on the EU market or used against a real client.
