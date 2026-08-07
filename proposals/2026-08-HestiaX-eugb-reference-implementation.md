## Development Fund Proposal: EuGB-on-Canton, an Open-Source Reference Implementation of European Green Bond Workflows

| Field | Value |
| :---- | :---- |
| Author | OleksandrZeziulinskyi |
| Org | HestiaX Ltd |
| Status | Draft |
| Created | 2026-08-07 |
| Label | `regulatory-compliance` |
| Review routing | Primary SIG: Regulatory Compliance (`regulatory-compliance`); technical co-review: Financial Workflows & Composability (`financial-workflows-composability`) |
| Champion | Needs Champion |

---

## Abstract

The European Green Deal Investment Plan aims to mobilise at least EUR 1 trillion in sustainable investment by 2030, with capital markets central to that mobilisation: Europe issued about USD 256 billion of green bonds in the first three quarters of 2025, representing 55% of global volume. But without verifiable evidence, a green label is only a claim. Regulation (EU) 2023/2631 establishes the proof framework for the European Green Bond (EuGB) designation through Taxonomy-based allocation of proceeds, independently reviewed disclosures, allocation and impact reporting, publication and regulatory notifications. This evidence turns a green claim into a credible instrument that institutional investors can evaluate and trust.

The public repositories and products reviewed for this proposal did not reveal a maintained open Canton implementation covering versioned EU Green Bond evidence, party-controlled review, privacy tests and instrument-to-evidence linkage. This grant delivers the missing public artifact.

HestiaX Ltd proposes an Apache-2.0 reference implementation of the EU Green Bond workflow on Canton. The implementation records evidence and attributed decisions, but does not determine legal compliance. The output is public infrastructure: every deliverable is reusable by any Canton participant and independently runnable.

HestiaX Ltd is an approved Canton MainNet validator operator and a member of the Canton ecosystem. Its two founders, senior engineers Oleksandr Zeziulinskyi and Sergii Dotsenko, deliver the funded scope full time over six months. The request is up to 2,000,000 CC: 1,500,000 CC for engineering, up to 200,000 CC ring-fenced for Committee-approved independent reviews, and up to 300,000 CC as an external-outcome incentive payable in two independent 150,000 CC tranches, one for external technical reuse and one for an institutional-domain evaluation.

---

## Specification

### 1. Objective

Deliver a public, Apache-2.0 reference implementation for the disclosure-evidence workflow of a non-sovereign, non-securitisation use-of-proceeds EU Green Bond, covering a corporate and project-finance scenario. The implementation will let Canton teams evaluate authorization, privacy, evidence versioning, statutory timing and instrument linkage.

On release, an unaffiliated team can clone and build from documented commands, run the EuGB conformance profile, execute the issuer and external-reviewer workflows through the Java/gRPC CLI, follow the scripted walkthrough, link evidence versions to a Token Standard V2 reference instrument, and test the visibility of on-ledger confidential evidence.

Version 1 excludes sovereign-specific workflows, securitisation, prospectus production, legal opinions, underwriting, KYC/AML, placement, distribution, custody, production payment rails and SPV structuring.

### 2. Implementation Mechanics

One public monorepo:

```text
spec/            normative schemas, field dictionary, canonicalization
regulatory/      source inventory, gap-search log and control profile
daml/            core, evidence, taxonomy, review, allocation, impact,
                 programme and conformance packages
sdk/java/        generated bindings, client and CLI
apps/            scripted evaluation walkthrough
conformance/     synthetic fixtures and expected results
infra/           reproducible local environment
docs/            architecture, privacy, integration, upgrade guides, specification
```

Architecture decisions of record:

- Daml is the authoritative authorization and workflow layer.
- Source documents stay off-ledger. Digests, provenance and workflow evidence go on-ledger.
- Java/gRPC CLI is the reference command and walkthrough path.
- Approved Canton Token Standard V2 (CIP-0112) is the primary token boundary, with a documented CIP-0056 V1 compatibility scenario.
- Daml Finance is not a version 1 dependency.
- Toolchain, DAR and package versions are pinned against measured artifacts.
- The software never exposes a legal-oracle field such as `isLegallyCompliant`.
- Every scoped deterministic control and authorization rule carries positive and negative conformance evidence.

The documentation states that the regulation-to-control mapping is an engineering artifact rather than legal advice and that the official text of Regulation (EU) 2023/2631 governs.

**Implementation status, in plain terms.** Pre-grant work comprises the domain model, party and authorization design, package boundaries, the off-ledger/on-ledger split, a regulation-to-control mapping, and a catalogue of more than 80 conformance scenarios covering positive, boundary, negative, privacy and upgrade behaviour. Milestone 1 converts these artifacts into a tested vertical slice.

Each participant acts through its own Canton party. The issuer records a factsheet digest and structured evidence; the external reviewer receives disclosure of the corresponding evidence version and alone controls recording its conclusion. Contracts compute statutory dates and record whether actions occur on time; off-ledger automation and publication remain outside scope. Corrections and supersessions preserve prior versions. Tests cover authorized and unauthorized transitions, boundary dates, privacy and upgrade behaviour.

### 3. Architectural Alignment

**App Building and Developer Experience:** importable Daml packages for a regulated multi-party workflow; interoperability through the approved Canton Token Standard; a minimal Java integration path; reusable conformance fixtures; public documentation and a worked example.

**Security and Resilience:** attributable, tamper-evident workflow evidence with authorization enforced on-ledger; explicit unrelated-party non-disclosure tests; published independent reviews; pinned versions, signed releases and reproducible builds.

The evidence packages carry no Token Standard dependency, so the evidence lifecycle is reusable with a different instrument implementation. The project re-implements no token primitives and replaces no existing Canton component.

### 4. Backward Compatibility

New packages only. Schemas and package names follow semantic versioning; CI runs Daml upgrade checks against prior released DARs; breaking data-model changes require a migration note or a new package name. Token Standard dependencies are isolated in the adapter package.

---

## Milestones and Deliverables

Six months from grant commencement; four milestones at 1.5-month intervals; both engineers full time throughout (1.5 engineer-months each per milestone; 3.0 per milestone; 12.0 total). Each engineering payment follows Committee acceptance of the specified evidence. The external-outcome incentive of up to 300,000 CC sits outside the engineering budget and is not a labour cost. Changes to target dates or scope require written Committee approval.

Milestone 1 closes with an architecture and toolchain go/no-go before later work begins. The funded core is the defined version 1 EuGB evidence workflow, Daml authorization and privacy, the Java CLI, reproducible local and CI execution, Token Standard V2 linkage, conformance evidence and documentation. If the baseline cannot support that scope, the Committee may revise, pause or close the grant; unreleased amounts return to the Fund.

### Milestone 1: Deterministic baseline and runnable vertical slice

| Field | Value |
|---|---|
| Estimated delivery | End of month 1.5 |
| Ecosystem value | Public artifacts and a runnable first workflow for outside inspection |
| Effort | Oleksandr 1.5 + Sergii 1.5 = 3.0 engineer-months |
| Deliverables | Public repository with reproducible build and CI; JSON schemas, field dictionary and canonicalization rules (RFC 8785 + SHA-256) with language-neutral golden fixtures; documented gap-search scope and results; regulation-to-control matrix; exact toolchain and package locks; core and evidence Daml packages; runnable issuer-to-reviewer vertical slice |
| Engineering payment | 375,000 CC |
| Delivery acceptance evidence | Architecture and toolchain go/no-go recorded; the build and vertical slice reproduce from documentation alone on a clean machine in public CI; published schemas and fixtures are consumable as released artifacts; no unpinned mandatory dependency |

### Milestone 2: Core EuGB workflow and Java integration

| Field | Value |
|---|---|
| Estimated delivery | End of month 3 |
| Ecosystem value | Defined version 1 evidence workflow plus a runnable Java/gRPC integration path |
| Effort | Oleksandr 1.5 + Sergii 1.5 = 3.0 engineer-months |
| Deliverables | Taxonomy-assessment, review, allocation, impact and programme Daml packages; publication and notification evidence; Java bindings, command client and CLI; positive, boundary, negative and privacy fixtures; local end-to-end scenario in CI |
| Engineering payment | 375,000 CC |
| Delivery acceptance evidence | CI enumerates funded choices and confidential contract types and fails when either lacks its required authorization or non-disclosure test; schemas exclude raw source documents and credentials from on-ledger payloads; the Java CLI scenario runs end to end in CI |

### Milestone 3: Token Standard interoperability, conformance and independent review

| Field | Value |
|---|---|
| Estimated delivery | End of month 4.5 |
| Ecosystem value | Instrument-to-evidence linkage on the approved standard, with third-party assurance under way against a feature-complete codebase |
| Effort | Oleksandr 1.5 + Sergii 1.5 = 3.0 engineer-months |
| Deliverables | Token Standard V2 evidence adapter and linkage scenario using a reference instrument; scripted evaluation walkthrough; canonical-hash golden-vector verification; full conformance suite green against the published scenario catalogue; both independent reviews commissioned against the Milestone 2 tag, with fieldwork complete and draft findings received |
| Engineering payment | 375,000 CC |
| Delivery acceptance evidence | The reference instrument links to exact factsheet and review versions; the Token Standard V2 linkage scenario passes; token holders gain no visibility into confidential evidence; both reviewers confirm in writing that fieldwork is complete and draft findings are delivered |

### Milestone 4: v1.0 release, external use and handover

| Field | Value |
|---|---|
| Estimated delivery | End of month 6 |
| Ecosystem value | A maintained, signed v1.0 that an institution can cite in an internal evaluation, with external use and evaluation demonstrated by organizations unaffiliated with HestiaX |
| Effort | Oleksandr 1.5 + Sergii 1.5 = 3.0 engineer-months |
| Deliverables | Signed `v1.0.0` with SBOM and checksums; architecture, privacy, integration and upgrade guides; final conformance report; both independent review reports published, with remediation and re-test completed; maintenance and vulnerability-disclosure policy; Committee-agreed external-outcome evidence templates |
| Engineering payment | 375,000 CC, plus up to 300,000 CC external-outcome incentive (two independent 150,000 CC tranches, defined under Funding) |
| Delivery acceptance evidence | Signed release; a Committee-designated engineer independently executes the conformance run against the released version; independent reproduction without HestiaX infrastructure; no unresolved Critical or High security finding and no material unresolved mapping defect; all funded artifacts under Apache-2.0 |

---

## Acceptance Criteria

The acceptance criteria combine technical delivery with independently verified ecosystem use:

1. **Public good:** no deliverable requires a HestiaX service, party, endpoint, API, licence or commercial relationship; the release, including specifications, the regulatory mapping and all documentation, is Apache-2.0 in its entirety.
2. **Regulatory fidelity and boundary:** all scenarios in the published scenario catalogue, including day-270, day-60 and 15% flexibility scenarios, produce their specified result in CI. Changes to the catalogue require a recorded specification change. The implementation records evidence and attributed decisions but does not determine whether a bond, issuer or programme complies with the Regulation.
3. **Independent reproducibility:** an unaffiliated engineer reproduces the build and the full local end-to-end scenario from documentation alone; the documented path targets completion within one working day, and the published report records actual elapsed time.
4. **Authorization and privacy:** CI enumerates funded choices and confidential contract types and fails when either lacks its required authorization-negative or unrelated-party non-disclosure test.
5. **Interoperability:** the evidence adapter and linkage scenario use approved Token Standard V2 interfaces.
6. **Integration:** the Java CLI executes the defined version 1 workflow through the Ledger API and produces a conformance report.
7. **External use and evaluation:** an unaffiliated organization imports a funded package into a prototype or pilot, adapts it, or completes the full integration exercise; an institutional-domain organization completes the structured evaluation defined under Funding. Stars, downloads, page views and testimonial quotes are not evidence.
8. **Maintainability and assurance:** pinned versions, upgrade checks, signed release, SBOM, a security-disclosure policy, and the published two-track independent review with no unresolved Critical or High finding and no material unresolved mapping defect.

### Public-good boundary

| Grant-funded public infrastructure (Apache-2.0) | HestiaX commercial activity (not funded, not required) |
|---|---|
| EuGB schemas, identifiers, canonicalization rules | Origination, structuring and advisory services |
| Daml evidence packages and reviewer workflow | Hosted issuance, administration or custody services |
| Conformance fixtures and scenario catalogue | Project pipeline and counterparty relationships |
| Java CLI and scripted walkthrough | Commercial APIs, SLAs and production operations |
| Documentation and release artifacts | Any hosted API, party or endpoint |

---

## Funding

### Total funding request

**Up to 2,000,000 CC**, in three separately governed components:

| Component | Amount | Nature |
|---|---:|---|
| Engineering | 1,500,000 CC | 12.0 senior engineer-months at 125,000 CC per engineer-month |
| Independent review (ring-fenced) | up to 200,000 CC | paid to Committee-approved third-party reviewers |
| External-outcome incentive | up to 300,000 CC | two independent 150,000 CC tranches, not a labour cost |
| **Total** | **up to 2,000,000 CC** | |

HestiaX contributes the pre-grant architecture, regulatory mapping and does not request reimbursement for that work.

### Payment breakdown by milestone

| Milestone | Engineering payment | External-outcome incentive |
|---|---:|---:|
| M1: deterministic baseline and runnable vertical slice | 375,000 CC | – |
| M2: core EuGB workflow and Java integration | 375,000 CC | – |
| M3: Token Standard interoperability, conformance and independent review | 375,000 CC | – |
| M4: v1.0 release, external use and handover | 375,000 CC | up to 300,000 CC |
| Independent review, both tracks (ring-fenced ceiling) | up to 200,000 CC | – |
| **Total** | **up to 1,700,000 CC** | **up to 300,000 CC** |

Every milestone funds 3.0 engineer-months, so the engineering payment is flat across milestones by construction.

### Cost basis

The engineering request funds **12.0 senior engineer-months**: two named engineers, full time, six months, at **125,000 CC per engineer-month** (1,500,000 ÷ 12.0).

The rate is fully loaded: tooling, infrastructure and administration are included, with no separate add-on. At a reference price of $0.094/CC, the average of CoinGecko and CoinMarketCap on 2026-08-08, the engineering request is approximately $141,000 and the maximum total approximately $188,000. Those USD figures are context only.

### External-use and evaluation component (300,000 CC)

The contingent amount is payable only on two independent 150,000 CC external outcomes:
1. **Canton technical reuse.** An organization unaffiliated with HestiaX imports a funded package into a prototype or pilot, adapts it, or completes the full integration exercise against the tagged release and supplies a reproducible report.
2. **Institutional-domain evaluation.** An issuer, arranger, appropriately authorized investment firm, bank or platform team, external reviewer, auditor, asset manager or comparable institutional-domain organization unaffiliated with HestiaX completes a structured technical, architecture or compliance evaluation against the tagged release and supplies a signed report identifying scope, environment, workflow and findings.

"Unaffiliated" means no shareholding, common control, employment or subcontracting relationship with HestiaX, and no commercial relationship in respect of the funded artifacts. Evidence may be public, redacted, or provided confidentially to the Committee where institutional policy prevents publication, but must be verifiable by the Committee. A meeting, marketing quote, memorandum of understanding, expression of interest, pipeline relationship, star, download or validator operation does not satisfy either outcome.

Evaluator categories and evidence forms are agreed with the Committee before grant commencement. The outcome window runs for 12 months from Milestone 4 acceptance; amounts not earned lapse to the Fund.

### Independent review (up to 200,000 CC, ring-fenced, two tracks)

The allocation covers two assurance tracks:
1. **Security and architecture:** Daml authorization correctness, Canton privacy and disclosure boundaries, package boundaries, off-ledger/on-ledger integrity, and upgrade safety.
2. **EuGB implementation mapping:** technical review of the implemented document fields, roles, deadlines and external-review boundaries against Regulation (EU) 2023/2631.

Reviewers must be independent of HestiaX, with demonstrated Daml/Canton security competence for the first track and EuGB or sustainable-finance implementation competence for the second. Before grant commencement, the Committee approves named reviewers, scopes, indicative quotes and the payment route. Both tracks are re-quoted at the start of Milestone 3 against the actual Milestone 2 codebase, within the approved ceiling. The allocation is capped at 200,000 CC; unused amounts are not disbursed. Milestone 3 requires fieldwork complete and draft findings received; Milestone 4 requires publication of both reports, plus remediation and re-test of Critical or High security findings and material mapping defects.

### Volatility stipulation

The duration is six months and all amounts are fixed in CC. HestiaX accepts the CIP-0100 default that the recipient carries CC price risk, both upside and downside. If Committee-requested scope changes extend the project beyond six months, the treatment of unearned remaining amounts, including whether significant USD/CC movement requires adjustment, is agreed with the Committee before the additional work begins.

---

## Co-Marketing

On release, HestiaX will coordinate with the Foundation on a release announcement and technical article and will make the walkthrough and external-use and evaluation results available for Foundation use.

---

## Motivation

### Why EU Green Bonds, and why now

The European Green Deal Investment Plan set out to mobilise at least EUR 1 trillion of sustainable investment over the decade, and most of that has to be raised in capital markets. Europe issued about USD 256 billion of green bonds in the first three quarters of 2025, roughly 55% of global volume, and the segment is structural rather than niche: green bonds reached 6.9% of all EU corporate and government bond issuance in 2024, up from 5.3% in 2023.

**Why the label exists.** A market that size cannot be run on self-declared greenness. A green label is credible only when backed by verifiable evidence. Regulation (EU) 2023/2631 establishes the European Green Bond designation, requiring proceeds to be allocated in full to eligible green activities, with at least 85% fully aligned with EU Taxonomy requirements and no more than 15% covered by a defined flexibility allowance; a pre-issuance factsheet independently reviewed before a bond is sold; allocation reports published until proceeds are fully allocated; at least one impact report; compliance with statutory publication and notification deadlines; and external reviewers supervised by ESMA. The label is a reporting and evidence obligation first and a marketing asset second, and it runs for the life of the bond.

**Why institutions recognize it.** More than 30 European green bonds totalling about EUR 30 billion have been issued since the standard became available in December 2024, from utilities, banks and municipalities as well as a sovereign issuer, and those issues have been consistently oversubscribed. For an institutional buyer, an internal risk function or an auditor, "European Green Bond" now names a known, supervised evidence package rather than a claim to be investigated one issuer at a time. That is what makes it the practical entry point for institutional adoption, and the reporting obligations are what an institution's reviewers actually examine.

**Where Canton stands.** Canton-connected platforms have been responsible for 57.5% of digital bond issuance by value since 2022 on Canton Network's own published figures, so the instrument side is well established. What those transactions tokenized was the bond. The disclosure evidence that the label consists of stayed off the ledger inside private systems: the factsheet and its independent review, the allocation and impact reports, the statutory deadlines and the attribution of every judgment. Canton therefore has the issuance record for the instrument and no public implementation of the label attached to it, which is precisely the artifact an institution's compliance, architecture and security reviewers ask to see before an evaluation can start. The solution this proposal funds is that artifact: an open, runnable, conformance-tested implementation of the EuGB evidence lifecycle on Canton, not another tokenization demonstration.

### What exists, and what is missing

| Layer | What exists | What is missing | What this proposal contributes |
|---|---|---|---|
| Issuer and arranger know-how | Mature issuance practice, frameworks, professional reviewers | Nothing is missing here, and this proposal adds nothing to it | Software modelling the workflow those professionals already run |
| Legal text and templates | Regulation (EU) 2023/2631, Annex templates, delegated acts | An implementation-level translation into schemas, deadlines, authorization rules | Ledger-neutral schemas; deterministic deadline and threshold calculations |
| Data standards | ICMA's Bond Data Taxonomy and comparable vocabularies | The multi-party workflow: who may attest what, when, visible to whom | Daml packages enforcing attribution, authorization and privacy |
| Digital-bond platforms | Proprietary platforms, several built on Daml/Canton | No maintained open EuGB workflow identified in the reviewed materials | An independently runnable Apache-2.0 reference |
| Canton infrastructure | Token Standard, Ledger API, developer tooling | A regulated-workflow reference built on them end to end | Evidence-to-instrument linkage and a runnable integration path |
| Conformance evidence | Vendor claims | Tests of authorized and unauthorized behaviour | An open suite of positive and negative conformance scenarios |

### Adoption: who uses it, concretely

| Participant | Current friction | Funded artifact | Evidence of use |
|---|---|---|---|
| Issuer | The EuGB evidence trail is re-assembled manually for each mandate | Programme, evidence, allocation and impact Daml packages with statutory clocks computed | Structured evaluation report (external outcome 2) |
| External reviewer | Document versions move by PDF and email with live-version ambiguity | Digest-bound review workflow with scoped disclosure; the conclusion remains reviewer-authored | Reviewer walkthrough of the review workflow |
| Custodian / tokenization platform | Bespoke, per-deal linkage between instrument and disclosure evidence | Token Standard V2 evidence-linkage pattern, reused without duplicating token infrastructure | Package import or conformance execution (external outcome 1) |
| Canton application / infrastructure team | Regulated-workflow plumbing rebuilt per project | Apache-2.0 packages, schemas, fixtures and conformance harness | Conformance execution in the team's own environment (external outcome 1) |

### Why this belongs in the Development Fund

The project delivers ecosystem infrastructure whose value depends on being publicly inspectable and independently reusable: an end-to-end authorization model spanning issuers and external reviewers, together with reusable conformance tests. Every deliverable will be released under Apache-2.0 from the first commit and usable by any Canton participant without HestiaX endpoints, fees, SLAs or a vendor relationship.

**Relationship to funded reference implementations.** The Fund has backed other Canton reference implementations, including Concordia, the settlement-pattern and reference DEX, and the OpenZeppelin ecosystem stack. This proposal focuses on a named EU disclosure regime with statutory documents, attesting roles, deadlines and a supervised external reviewer.

---

## Rationale

**Why Canton and Daml.** EuGB workflows involve exact document versions, attributable approvals, confidential evidence and independent institutions acting under their own authority. Canton provides sub-transaction privacy, while Daml signatories and controllers ensure that only the designated reviewer party can record its conclusion. Synchronized state reduces off-ledger reconciliation of allocation totals, review states and deadlines. Where the Regulation requires professional judgment, the software records and attributes that judgment rather than simulating it.

**Why Java; why no Daml Finance.** Java is the institutional integration reality; the Java CLI demonstrates the generated-bindings and Ledger API path without introducing a hosted service or application-owned database. The evidence workflow requires no financial-contract framework and therefore carries no Daml Finance dependency.

---

## Delivery Team

**Canton commitment.** HestiaX is a member of the Canton ecosystem, was approved as a validator operator in the Global Synchronizer Foundation's batch approval and runs its MainNet validator as company-operated infrastructure.

**Engineering ownership.**

| Person | Primary ownership |
|---|---|
| Oleksandr Zeziulinskyi | Product and technical lead; EuGB implementation profile; Daml architecture and implementation; Token Standard and evidence linkage; conformance scenarios; institutional validation and external engagement |
| Sergii Dotsenko | Distributed-systems and integration lead; Java/gRPC CLI; security hardening; CI and release engineering; shared Daml implementation and performance work |

Oleksandr Zeziulinskyi has worked in software engineering for 18 years and in blockchain infrastructure since 2014, including at KnCMiner, Spotify and Scania. His public GitHub record includes contributions to the beaconcha.in Ethereum consensus explorer.

Sergii Dotsenko is a senior Java and distributed-systems engineer with 20 years of experience, including work at Barclays, Sony Mobile, Ericsson and Avid. His expertise includes Java and JVM performance, networking and cybersecurity.

Both engineers work full time for the six-month grant, share Daml implementation, and reserve time for independent-review remediation and evaluator support. Independent reviewers are separately procured third parties.


**Public-good incentive.**

Reusable ecosystem infrastructure, licensed under Apache-2.0 and available to all adopters.

---

## Maintenance and Sustainability

For 12 months after v1.0, HestiaX allocates up to 30 engineer-days to compatibility assessment and corrective releases for supported Canton and Token Standard versions, security fixes, public issue and pull-request triage and migration notes for breaking changes.

If stewardship ends, HestiaX will offer transfer of the Apache-2.0 repository to a Foundation-controlled organization with 60 days' notice. Contributions are accepted under the Developer Certificate of Origin.

The evidence layer is instrument-independent by construction: the reusable asset is the pattern rather than the EuGB profile itself, so any regime with named documents, attesting roles, statutory deadlines and an independent reviewer can be expressed as a further profile over the same packages. No such extension is scoped or funded here.

---

## Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Stale or divergent read models | No application-owned read model in version 1; the Java CLI reads through the Ledger API, so ledger state is authoritative |
| Shared-network availability | Version 1 targets a reproducible local and CI environment; no deliverable depends on shared-network access |
| Two-person capacity | Both engineers work full time, share Daml implementation, and begin with the documented design artifacts; Milestone 1 provides a go/no-go before later work |
| Independent-review timeline | Named reviewers, scopes and quotes are approved before grant commencement; remediation time is reserved |
| No committed external evaluator at proposal date | One conditional technical-reuse commitment and one institutional-evaluation commitment are obtained before grant commencement; 300,000 CC remains contingent on completed outcomes |
| Token Standard package pins invalidated by a Splice upgrade | Pins bind to a measured Splice release; an upgrade requires version-lock regeneration rather than a scope change |
| Legal-automation overclaim | The implementation records evidence and attributed decisions rather than legal conclusions; the mapping review tests this boundary |
| Private evidence leakage | Raw documents and credentials are excluded from ledger payloads; on-ledger visibility has unrelated-party tests and independent review; source-document delivery is outside scope |


---

## References

**Legal and regulatory:**

- Regulation (EU) 2023/2631 (European Green Bonds): `https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32023R2631`
- European Commission, EuGB delegated and implementing acts: `https://finance.ec.europa.eu/regulation-and-supervision/financial-services-legislation/implementing-and-delegated-acts/european-green-bond-standard-regulation_en`
- European Commission, "Shaping a sustainable future: key updates on EU Green Bonds" (2026-03-19): `https://finance.ec.europa.eu/news/shaping-sustainable-future-key-updates-eu-green-bonds-2026-03-19_en`
- ESMA, register of external reviewers published (2026-06-22): `https://www.esma.europa.eu/press-news/esma-news/esma-publishes-register-external-reviewers-under-eugb-regulation`
- ICMA, Bond Data Taxonomy: `https://www.icmagroup.org/fintech-and-digitalisation/fintech-advisory-committee-and-related-groups/bond-data-taxonomy/`

**EU policy and market context:**

- European Commission, COM(2020) 21, Sustainable Europe Investment Plan / European Green Deal Investment Plan (mobilising at least EUR 1 trillion over the decade): `https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:52020DC0021`
- LSEG, "Green debt market passes $3 trillion milestone" (Europe at about USD 256 billion of green bond issuance in the first three quarters of 2025, roughly 55% of global volume): `https://www.lseg.com/en/insights/green-debt-market-passes-3-trillion-milestone`

**Canton Development Fund and standards:**

- Canton Development Fund repository and submission rules: `https://github.com/canton-foundation/canton-dev-fund`
- Development Fund Proposal Review Process: `https://github.com/canton-foundation/canton-dev-fund/blob/main/Development%20Fund%20Proposal%20Review%20Process.md`
- SIG directory: `https://github.com/canton-foundation/canton-dev-fund/blob/main/sig-directory.md`
- CIP-0100 (Development Fund governance, Champion and volatility rules): `https://github.com/canton-foundation/cips/blob/main/cip-0100/cip-0100.md`
- Canton Token Standard V2, CIP-0112: `https://github.com/global-synchronizer-foundation/cips/blob/main/cip-0112/cip-0112.md`
- Canton Token Standard V1, CIP-0056: `https://github.com/global-synchronizer-foundation/cips/blob/main/cip-0056/cip-0056.md`
- Canton Network, "Canton Network Accounts for Over Half of All Digital Bond Issuances" (2025-01-13): `https://www.canton.network/blog/canton-network-accounts-for-over-half-of-all-digital-bond-issuances`

**HestiaX and team:**

- Canton ecosystem directory, HestiaX: `https://www.cantonecosystem.com/ecosystem/hestiax`
- Global Synchronizer Foundation, validator operator batch approval naming HestiaX Ltd (2026-06-10), with the MainNet allocation record linked from it (archive link supplied in the PR discussion)
- HestiaX: `https://hestiax.org/`
- Oleksandr Zeziulinskyi, GitHub: `https://github.com/OleksandrZeziulinskyi`; LinkedIn: `https://www.linkedin.com/in/oleksandr-zeziulinskyi/`
- Sergii Dotsenko, GitHub: `https://github.com/sdotsenko`; LinkedIn: `https://www.linkedin.com/in/sdotsenko/`

**Market context:**

- Canton Coin price: CoinGecko (`https://www.coingecko.com/en/coins/canton`) and CoinMarketCap (`https://coinmarketcap.com/currencies/canton-network/`)
