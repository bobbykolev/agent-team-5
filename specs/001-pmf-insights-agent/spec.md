# Feature Specification: PMF Insights Agent

**Feature Branch**: `001-pmf-insights-agent`
**Created**: 2026-02-04
**Status**: Draft
**Input**: Build an agent that ingests feedback from multiple touchpoints (product, growth, sales, research, design, support), normalizes data with persona/pain-point taxonomy, synthesizes cross-source insights, scores PMF signals, and maps opportunities to testable experiments.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Ingest and Normalize Feedback Data (Priority: P1)

As a product team member, I want to submit feedback from any source so that all insights are captured in a unified format with consistent tagging.

The agent ingests data from multiple touchpoints: customer conversations, interviews, evaluations, sales conversations, events, usage analytics, research articles/papers, expert opinions, and decision diaries. Each piece of feedback is normalized into a common signal format and tagged with the shared persona and pain-point taxonomy.

**Why this priority**: This is the foundation. Without normalized, tagged data, no downstream analysis is possible. Every other capability depends on having a clean, unified data pool.

**Independent Test**: Can be fully tested by submitting sample feedback from each source type and verifying signals are created with correct persona/pain-point tags.

**Acceptance Scenarios**:

1. **Given** a customer interview transcript, **When** the agent ingests it, **Then** the system creates one or more signals tagged with relevant personas and pain points
2. **Given** usage analytics data, **When** the agent ingests it, **Then** behavioral patterns are converted to signals with appropriate tags
3. **Given** feedback that doesn't match existing taxonomy, **When** the agent ingests it, **Then** it flags the signal for taxonomy review while still storing it
4. **Given** a source requiring consent, **When** data is ingested, **Then** the system verifies consent status before processing

---

### User Story 2 - Generate Weekly Persona Insights (Priority: P2)

As a product leader, I want to receive weekly synthesized insights per persona so that I understand the current state of each customer segment without manually reviewing all feedback.

The agent runs scheduled synthesis jobs that pull signals from all sources and generate persona-aligned themes. Each weekly report shows overlaps and gaps across teams, highlighting what multiple sources agree on versus contradictions.

**Why this priority**: Once data is normalized (P1), synthesis is the core value proposition. Weekly cadence ensures continuous insight flow without overwhelming stakeholders.

**Independent Test**: Can be fully tested by populating the system with tagged signals and triggering a synthesis run, then verifying the output contains persona-specific themes with source attribution.

**Acceptance Scenarios**:

1. **Given** signals from multiple sources for a persona, **When** weekly synthesis runs, **Then** the agent produces a report with themes ranked by signal frequency and source diversity
2. **Given** conflicting signals across sources, **When** synthesis runs, **Then** the report highlights the contradiction with evidence from each source
3. **Given** no new signals for a persona, **When** synthesis runs, **Then** the report indicates data staleness and recommends outreach

---

### User Story 3 - Score PMF Progress (Priority: P3)

As a product strategist, I want to see PMF scores by persona and pain point so that I can track whether we're making progress toward product-market fit and prioritize accordingly.

The agent analyzes signals and computes PMF scores that indicate how well the product addresses each persona's pain points. Scores update as new signals arrive, showing trends over time.

**Why this priority**: PMF scoring transforms raw insights into actionable metrics. Depends on P1 (data) and benefits from P2 (synthesis context) but can be built and tested independently.

**Independent Test**: Can be fully tested by submitting a set of positive and negative signals for a persona/pain-point combination and verifying the score reflects the balance.

**Acceptance Scenarios**:

1. **Given** a mix of positive and negative signals for a persona-pain-point pair, **When** PMF scoring runs, **Then** the score reflects the weighted sentiment
2. **Given** a trend of improving signals over time, **When** viewing PMF scores, **Then** the trend is visible with directional indicator
3. **Given** a pain point with insufficient signals, **When** viewing PMF scores, **Then** confidence level is marked as low with data collection recommendation

---

### User Story 4 - Map Opportunities to Experiments (Priority: P4)

As a product manager, I want to see top opportunities mapped to testable experiments so that I can make evidence-based decisions about what to build next.

The agent identifies gaps between persona needs and current product capabilities, ranks opportunities by potential impact, and suggests experiments with validation criteria.

**Why this priority**: This is the decision-support layer that drives action. Requires P1-P3 to be meaningful but delivers the ultimate business value of the system.

**Independent Test**: Can be fully tested by creating a scenario with clear unmet needs and verifying the agent proposes relevant experiments with measurable success criteria.

**Acceptance Scenarios**:

1. **Given** a high-frequency pain point with low PMF score, **When** opportunity mapping runs, **Then** the agent surfaces it as a top opportunity with suggested experiments
2. **Given** a suggested experiment, **When** viewing details, **Then** the experiment includes validation criteria, effort estimate category (small/medium/large), and rollout recommendation
3. **Given** multiple opportunities, **When** viewing the opportunity map, **Then** they are ranked by impact-to-effort ratio with justification

---

### User Story 5 - Generate On-Demand Executive Briefs (Priority: P5)

As an executive, I want to request ad-hoc briefs on specific topics so that I can quickly get context for strategic decisions or stakeholder communications.

The agent generates focused briefs on demand, pulling relevant signals and synthesis from the data pool. Briefs can be scoped by persona, pain point, time range, or custom query.

**Why this priority**: On-demand capability adds flexibility but is not essential for core PMF tracking workflow. Valuable for executive engagement but secondary to automated insights.

**Independent Test**: Can be fully tested by requesting a brief on a specific persona and verifying it contains relevant signals, themes, and recommendations.

**Acceptance Scenarios**:

1. **Given** a request for a persona-specific brief, **When** the agent generates it, **Then** the brief contains recent signals, key themes, PMF score, and recommended actions
2. **Given** a "what-if" scenario request, **When** the agent processes it, **Then** it projects impact based on historical signal patterns
3. **Given** a brief request for a topic with no data, **When** generated, **Then** the brief clearly states data gaps and suggests sources to collect

---

### Edge Cases

- What happens when a signal matches multiple personas? System tags with all relevant personas and tracks multi-persona signals separately for cross-segment analysis.
- How does the system handle conflicting signals from the same source? Both are preserved with timestamps; synthesis highlights internal source contradictions.
- What happens when the taxonomy needs to evolve? New personas/pain-points can be added; existing signals can be re-tagged with governance approval.
- How does the system handle data sources that go offline? Marks source as stale, continues with available data, alerts administrators.
- What happens when consent is revoked for a source? All signals from that source are anonymized or removed based on governance policy.

## Requirements *(mandatory)*

### Functional Requirements

**Data Ingestion & Normalization**

- **FR-001**: System MUST ingest data from: customer conversations, customer interviews, customer evaluations, sales conversations, event feedback, usage analytics, research articles, research papers, expert opinions, and decision diaries
- **FR-002**: System MUST normalize all ingested data into a common signal format with: source identifier, timestamp, raw content, normalized summary, confidence score
- **FR-003**: System MUST tag each signal with zero or more personas from the shared taxonomy
- **FR-004**: System MUST tag each signal with zero or more pain points from the shared taxonomy
- **FR-005**: System MUST flag signals that don't clearly match existing taxonomy for human review
- **FR-006**: System MUST track data lineage showing source, ingestion time, and transformation history for each signal

**Taxonomy Management**

- **FR-007**: System MUST maintain a shared persona taxonomy with: name, description, key characteristics, related pain points
- **FR-008**: System MUST maintain a shared pain-point taxonomy with: name, description, severity level, related personas
- **FR-009**: System MUST allow authorized users to add, modify, or deprecate taxonomy entries with versioning

**Synthesis & Analysis**

- **FR-010**: System MUST generate weekly synthesis reports per persona showing themes, signal counts, and source distribution
- **FR-011**: System MUST generate cross-persona synthesis highlighting overlaps and unique needs
- **FR-012**: System MUST identify and highlight conflicting signals across sources
- **FR-013**: System MUST compute PMF scores per persona-pain-point combination based on signal sentiment and frequency

**Opportunity & Experiment Planning**

- **FR-014**: System MUST identify opportunities by comparing persona needs against current capabilities
- **FR-015**: System MUST rank opportunities by estimated impact and required effort
- **FR-016**: System MUST suggest testable experiments for top opportunities with validation criteria

**On-Demand Capabilities**

- **FR-017**: System MUST generate executive briefs on demand with customizable scope (persona, pain point, time range)
- **FR-018**: System MUST support "what-if" scenario analysis projecting impact of proposed changes
- **FR-019**: System MUST generate curated research briefs pulling relevant external research

**Operations**

- **FR-020**: System MUST run scheduled ingestion jobs at configurable intervals
- **FR-021**: System MUST run daily PMF score refreshes
- **FR-022**: System MUST run weekly synthesis generation automatically
- **FR-023**: System MUST update decision diaries when significant insights or decisions are recorded

**Privacy & Governance**

- **FR-024**: System MUST track explicit consent status per data source
- **FR-025**: System MUST anonymize data where feasible based on source requirements
- **FR-026**: System MUST enforce per-source visibility controls limiting who can see signals from specific sources
- **FR-027**: System MUST maintain complete data lineage for audit purposes
- **FR-028**: System MUST support consent revocation with appropriate data handling (anonymization or deletion)

### Key Entities

- **Signal**: A normalized piece of feedback or insight; contains source reference, timestamp, content summary, persona tags, pain-point tags, sentiment indicator, confidence score
- **Persona**: A customer archetype in the shared taxonomy; contains name, description, characteristics, associated pain points, creation date, version
- **Pain Point**: A problem or need in the shared taxonomy; contains name, description, severity, associated personas, creation date, version
- **Source**: A data origin with consent and visibility rules; contains type, name, consent status, visibility scope, last ingestion date, connection status
- **Theme**: A synthesized pattern across multiple signals; contains description, supporting signals, confidence, persona association, time range
- **PMF Score**: A metric tracking product-market fit progress; contains persona reference, pain-point reference, score value, trend direction, confidence level, last updated
- **Opportunity**: An identified gap or unmet need; contains description, impact estimate, effort estimate, supporting signals, suggested experiments
- **Experiment**: A testable hypothesis with validation plan; contains description, hypothesis, success criteria, effort category, rollout recommendation, linked opportunity

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Product team members can submit feedback from any supported source type and see it normalized within 5 minutes
- **SC-002**: 90% of ingested signals are automatically tagged with at least one persona and one pain point without manual intervention
- **SC-003**: Weekly synthesis reports are generated automatically and delivered to stakeholders without manual trigger
- **SC-004**: Stakeholders report spending 50% less time manually aggregating feedback across sources
- **SC-005**: PMF scores update within 24 hours of new signal ingestion
- **SC-006**: 80% of suggested experiments are rated as "actionable" by product managers
- **SC-007**: Executive briefs can be generated on-demand within 2 minutes of request
- **SC-008**: All signals maintain complete data lineage from source to insight
- **SC-009**: System correctly enforces consent and visibility rules with zero unauthorized data exposure
- **SC-010**: Cross-source contradictions are surfaced in 100% of synthesis reports where they exist

## Assumptions

- The company has existing data in the specified source systems that can be accessed programmatically or via structured export
- Stakeholders have agreed on an initial persona and pain-point taxonomy (can evolve over time)
- There is organizational alignment on what constitutes a "positive" vs "negative" signal for PMF scoring
- Consent tracking for existing data sources has been established or will be established as part of initial setup
- Users have appropriate permissions/roles defined for accessing different sensitivity levels of data
