# Response to Vittal's EU AI Act audit of AI Pre-Scan

**Builder:** Ugo Ahukannah · **Auditor:** Vittal Navale · **Returned:** 20 August 2026
His request: `04-exchange-request-to-ugo_EU AI Act.md` · His report:
[`from_vittal/02-audit-report_EU AI Act.md`](./from_vittal/02-audit-report_EU%20AI%20Act.md) ·
His questions: [`from_vittal/01-clarifying-questions-log_EU AI Act.md`](./from_vittal/01-clarifying-questions-log_EU%20AI%20Act.md)

*(Note: the two relative links above point into Ugo's own repository structure, where he keeps my materials under `from_vittal/`. In this repository the equivalent files are [`02-audit-report_EU AI Act.md`](02-audit-report_EU%20AI%20Act.md) and [`01-clarifying-questions-log_EU AI Act.md`](01-clarifying-questions-log_EU%20AI%20Act.md), kept as-received below.)*

**Sequencing, stated honestly.** Unlike the GDPR exchange — where I wrote my answers before opening
your report — **I had already read your AI Act report before writing this**, because it arrived
alongside the file I needed for my own Whizbiz debrief. So Part 2 is not a blind reveal. What I can
show instead is that the positions in it are not retrofitted: they are transcribed from my own EU AI
Act self-audit, **committed as `5b9b18d` on 19 August 2026**, before your materials arrived. The git
history is the evidence; I would not ask you to take my word for it.

---

## Part 1 — Answers to your five questions

**Q1. Is AI Pre-Scan intended for market placement, or is it a tool the builder alone runs?**
> **Market placement is the intent, so your provider framing is the right one.** It has not been
> placed on any market and has no external users today, so no provider obligation has crystallised —
> but the documentation designs for an external adviser throughout, and treating that as a thought
> experiment would be a convenient reading rather than an honest one. My own self-audit reached the
> same place: provider is **prospective**, and the obligations are real the moment it ships.

**Q2. Does a finding ever function as an assessment of an identifiable person, or is every finding
strictly about the organisation?**
> **Strictly about the organisation, and enforced rather than intended.** The report spec restricts
> the subject to the company, and the schema forbids regime vocabulary. Named individuals appear only
> inside quoted source text, incidentally. Your provisional assumption is correct — and it is the
> single fact that keeps the system out of Annex III, which is why I would not let it drift.

**Q3. What editorial step does the adviser apply before a report reaches a client?**
> **None that is defined, and your assumption is right.** The adviser reads the report and acts on
> it; nothing in the design requires her to rewrite or take editorial ownership of the findings, and
> the standing legal notice is deliberately not editable by any generation step. This is the question
> in your log I could not answer from my own documentation, and see Part 3 — it is also the one that
> produced the best finding in your report.

**Q4. Is a legal basis and transfer mechanism in place for OpenAI and Pinecone?**
> **Still open — your reading of `proposal.md` §9 is current, not stale.** Nothing has been signed or
> scheduled. Since your audit I have written the record that was missing: `docs/data-protection.md`
> now carries the processing inventory, a three-part LIA, the roles and processors, the transfers,
> and an Article 35(1) threshold assessment, with the open items given owners and triggers. The gap
> is unchanged; what changed is that it is now tracked rather than mentioned.

**Q5. Who holds provider responsibility — the individual or an entity?**
> **Undecided, and no entity exists.** For this audit, provider = me as project author, exactly as
> you assumed. My own self-audit records the same fork and does not resolve it. It needs deciding
> before the first external user rather than at the point of a dispute.

---

## Part 2 — My position, from the self-audit committed before your report

**My first-pass tier: minimal risk.** Justification as written on 19 August: the system assesses
**organisations rather than natural persons**, which keeps it outside Article 5 and every Annex III
area, and its Article 50(1) disclosure duty is discharged by context — so no tier-specific
obligations attach beyond those applying to every AI system.

**My top three findings (of five):**
> 1. **Personal data reached the evidence store despite a policy saying it did not** — whole fetched
>    pages chunked, embedded and stored in a US vector database, with no deletion path anywhere.
>    Policy without a control. *(Fixed on 19 August, commit `3eee7f6`: validated passages only, plus
>    `delete_namespace` and `purge_scan`, purging before every live scan.)*
> 2. **No Article 4 AI literacy material** — nothing tells a user what the tool cannot conclude, or
>    that measured recall of 0.444 means absence of a finding is not evidence of absence.
> 3. **Provider status undecided while the product is designed for a subscription** — a paperwork gap
>    now, an accountability gap at launch.

---

## Part 3 — After reading your report

**Our tiers differ, and yours is the better call.** You classified AI Pre-Scan as **limited risk**,
on the ground that GPT-4o generates the report text and it reaches a third party — the adviser's
client — without a human-editorial step that would qualify for the Article 50(4) exemption, so
**Article 50(2)** synthetic-content marking applies. I classified it **minimal risk** and considered
only Article 50(1), the chatbot-style disclosure. I never asked whether the *output* was synthetic
content.

**My initial disagreement, and where I landed.** My instinct was that the system does not *generate*
in the sense 50(2) targets — its outputs are extractions anchored to quoted source text, and 50(2)
exempts systems that do not substantially alter the input data or its semantics. Three things moved
me. First, the **discussion list is genuinely generated prose**, present in every report and the
entire deliverable when no evidence is found; the exemption cannot cover it. Second, Article 50
questions turn on what the recipient could reasonably believe, and a client receiving a polished
document from their adviser cannot distinguish extraction from generation. Third — the part I would
have been slowest to concede — you read my own report-spec rule, that the legal notice is *"not
editable by any generation step"*, as evidence **against** the editorial-responsibility exemption. I
had always read that rule as a safeguard. Same sentence, opposite inference, and yours is the reading
a regulator would more plausibly take.

**Where I still hold ground:** severity and certainty, not existence. You class it Significant and
launch-blocking; I would keep it Significant while noting the remedy is a line of text plus a
documented review step, and that the exemption question is genuinely open rather than settled against
me. That is a narrower disagreement than the one I started with.

**What my self-audit caught that your report did not reach.** The whole-page storage defect and its
missing deletion path — invisible to you because it was fixed the same day you audited. Also the
measured over-claim rate of 0.333 as a *consulting* risk rather than a compliance one: the output
feeds a compliance determination, so being wrong one time in three is a professional-liability
exposure even where the AI Act is silent.

**What you caught that I had not seen.** Article 50(2) — the tier-setting question, and I missed it
entirely. Your Finding 5 was also fair: the currentness safeguard had been inert in production, and
tying that to the editorial-review question rather than treating it as a separate bug is the sharper
move. Your Finding 3 on the incidental-personal-data legal basis was right too, and led directly to
the LIA now recorded in `docs/data-protection.md`.

---

## Part 4 — Joint closing note

**My half, for you to amend or replace:**
> The difference between auditing your own work and auditing someone else's is which question you
> forget to ask. I audited AI Pre-Scan against the tier I already believed it occupied and checked
> that belief thoroughly — Article 5, every Annex III area, Article 50(1) — and never asked whether
> the thing it produces is itself regulated content. You had no belief to defend, so Article 50(2)
> was simply visible. The mirror held in the other direction: I could see that Whizbiz holds a
> customer's name and address with no notice path, and could not see that German invoice law makes
> those fields mandatory. Each of us was blind precisely where the other had nothing invested.

**Agreed final version:**
> _yours to close — amend mine or replace it, and I will take whatever you land on as final._
