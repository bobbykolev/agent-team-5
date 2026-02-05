# Feature Specification: PMF Insights Agent

**Feature Branch**: `001-pmf-insights-agent`
**Created**: 2026-02-04
**Status**: Draft
**Input**: Build an agent that ingests feedback from multiple touchpoints (product, sales, research, design, support), normalizes data with persona/pain-point taxonomy, synthesizes cross-source insights, scores PMF signals, and maps opportunities and recommendations for product development priorities.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Ingest and Normalize Feedback Data (Priority: P1)

As a team member, I want to submit feedback from any source so that all insights are captured in a unified format with consistent tagging.

The agent ingests data from multiple touchpoints: customer conversations, interviews, evaluations, sales conversations, events, usage analytics, research articles/papers, expert opinions, and decision diaries. Each piece of feedback is normalized into a common signal format and tagged with the shared persona and pain-point taxonomy.

**Why this priority**: This is the foundation. Without normalized, tagged data, no downstream analysis is possible. Every other capability depends on having a clean, unified data pool.

**Independent Test**: Can be fully tested by submitting sample feedback from each source type and verifying signals are created with correct persona/pain-point tags.

**Acceptance Scenarios**:

1. **Given** a customer interview transcript, **When** the agent ingests it, **Then** the system creates one or more signals tagged with relevant personas and pain points
2. **Given** usage analytics data, **When** the agent ingests it, **Then** behavioral patterns are converted to signals with appropriate tags
3. **Given** feedback that doesn't match existing taxonomy, **When** the agent ingests it, **Then** it flags the signal for taxonomy review while still storing it
4. **Given** a source requiring consent, **When** data is ingested, **Then** the system verifies consent status before processing
5. **Given** a data source with missing required metadata, **When** the agent attempts ingestion, **Then** the system rejects those data points and logs specific validation errors
6. **Given** authorized user corrections to signal tags, **When** feedback is submitted, **Then** the correction is applied immediately and queued for write-back to the source system

---

### User Story 2 - Generate Weekly Persona Insights (Priority: P2)

As a product market fit lead and product team member, I want to receive weekly synthesized insights per persona so that I understand the current state of each customer segment without manually reviewing all feedback.

The agent runs scheduled synthesis jobs that pull signals from all sources and generate persona-aligned themes. Each weekly report shows overlaps and gaps across teams, highlighting what multiple sources agree on versus contradictions.

**Why this priority**: Once data is normalized (P1), synthesis is the core value proposition. Weekly cadence ensures continuous insight flow without overwhelming stakeholders.

**Independent Test**: Can be fully tested by populating the system with tagged signals and triggering a synthesis run, then verifying the output contains persona-specific themes with source attribution.

**Acceptance Scenarios**:

1. **Given** signals from multiple sources for a persona, **When** weekly synthesis runs, **Then** the agent produces a report with themes ranked by signal frequency and source diversity
2. **Given** conflicting signals across sources, **When** synthesis runs, **Then** the report highlights the contradiction with evidence from each source
3. **Given** no new signals for a persona, **When** synthesis runs, **Then** the report indicates data staleness and recommends outreach

---

### User Story 3 - Score PMF Progress (Priority: P3)

As a product market fit lead, I want to see PMF scores by persona and pain point so that I can track whether we're making progress toward product-market fit and prioritize accordingly.

The agent analyzes signals and computes PMF scores that indicate how well the product addresses each persona's pain points. Scores update as new signals arrive, showing trends over time.

**Why this priority**: PMF scoring transforms raw insights into actionable metrics. Depends on P1 (data) and benefits from P2 (synthesis context) but can be built and tested independently.

**Independent Test**: Can be fully tested by submitting a set of positive and negative signals for a persona/pain-point combination and verifying the score reflects the balance.

**Acceptance Scenarios**:

1. **Given** a mix of positive and negative signals for a persona-pain-point pair, **When** PMF scoring runs, **Then** the score reflects the weighted sentiment
2. **Given** a trend of improving signals over time, **When** viewing PMF scores, **Then** the trend is visible with directional indicator
3. **Given** a pain point with low signal volume, **When** viewing PMF scores, **Then** confidence level scales with signal count (higher confidence as volume approaches 100 signals) with data collection recommendation for low volumes

---

### User Story 4 - Generate Product Priority Recommendations (Priority: P4)

As a product manager, I want to receive recommendations for product development priorities and functionalities along with open validation questions so that I can make evidence-based decisions about what to build next and what to validate through customer interviews.

The agent analyzes gaps between persona needs and current product capabilities, generates prioritized recommendations for functionalities and solutions to develop, and identifies critical open questions that require validation through customer interviews, user research, or other discovery methods.

**Why this priority**: This is the decision-support layer that drives action and learning. Requires P1-P3 to be meaningful but delivers the ultimate business value by guiding both product development and validation efforts.

**Independent Test**: Can be fully tested by creating a scenario with clear unmet needs and verifying the agent proposes relevant development recommendations with associated validation questions.

**Acceptance Scenarios**:

1. **Given** a high-frequency pain point with low PMF score, **When** recommendation generation runs, **Then** the agent surfaces it as a top priority with specific functionality/solution recommendations
2. **Given** a product priority recommendation, **When** viewing details, **Then** the recommendation includes impact rationale, effort estimate category (small/medium/large), and a list of open questions requiring validation
3. **Given** multiple recommendations, **When** viewing the priority list, **Then** they are ranked by impact-to-effort ratio with clear justification based on signal patterns
4. **Given** open validation questions, **When** viewing them, **Then** each question includes suggested research method (e.g., customer interview, survey, usability test), priority level, and target personas to interview
5. **Given** signals that don't fit existing personas or pain points, **When** recommendation generation runs, **Then** the agent identifies potential taxonomy gaps and recommends new personas/pain points to add with supporting evidence from signals

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
- What happens when a data source changes its schema without notification? System detects schema mismatch during validation, suspends ingestion for that source, and alerts administrators with schema difference details.
- How does the system handle data sources with consistently poor quality? System calculates signal-to-noise ratio per source; if below 50% for 30 days, administrators are notified with data quality improvement recommendations.
- What happens when feedback submission to a source fails? Feedback is queued with exponential backoff retry; if source remains unavailable after 72 hours, administrators are notified and feedback is marked for manual intervention.

## Requirements *(mandatory)*

### Functional Requirements

**Data Ingestion & Normalization**

- **FR-001**: System MUST ingest data from: customer conversations, customer interviews, customer evaluations, sales conversations, event feedback, usage analytics, research articles, research papers, expert opinions, and decision diaries
- **FR-002**: System MUST normalize all ingested data into a common signal format with: source identifier, timestamp, raw content, normalized summary, confidence score
- **FR-003**: System MUST tag each signal with zero or more personas from the shared taxonomy
- **FR-004**: System MUST tag each signal with zero or more pain points from the shared taxonomy
- **FR-005**: System MUST flag signals that don't clearly match existing taxonomy for human review
- **FR-006**: System MUST track data lineage showing source, ingestion time, and transformation history for each signal

**Data Source Requirements**

- **FR-006a**: Each data source MUST provide data with the following minimum metadata: unique identifier, creation timestamp, content or description field
- **FR-006b**: Each data source SHOULD include optional enrichment metadata to improve signal quality: author/creator identifier, last modified timestamp, category or type classification
- **FR-006c**: System MUST validate data quality on ingestion, assigning quality scores based on: completeness of metadata (40%), presence of enrichment fields (20%), content clarity (20%), successful taxonomy matching (20%)
- **FR-006d**: System MUST reject data points missing required metadata: unique identifier, timestamp, or content field
- **FR-006e**: System MUST track health metrics per data source including: connection uptime, ingestion success rate, average signal quality score, signal-to-noise ratio, last successful ingestion timestamp
- **FR-006f**: System MUST alert administrators when data sources breach quality thresholds: connection failure for more than 2 hours, ingestion success rate below 90% over 24 hours, average signal quality below 60 for 3 consecutive cycles, or no new data for more than 48 hours
- **FR-006g**: System MUST allow authorized users to submit feedback corrections back to data sources when sources support write operations, including: tag corrections, content enrichment, and data quality flags
- **FR-006h**: System MUST retry failed feedback submissions using exponential backoff: 5 minutes, 15 minutes, 1 hour, 24 hours before marking for manual intervention

**Taxonomy Management**

- **FR-007**: System MUST maintain a shared persona taxonomy with: name, description, key characteristics, related pain points
- **FR-008**: System MUST maintain a shared pain-point taxonomy with: name, description, severity level, related personas
- **FR-009**: System MUST allow authorized users to add, modify, or deprecate taxonomy entries with versioning; AI-proposed additions require explicit human approval before integration
- **FR-009a**: System MUST identify potential new personas or pain points from signal patterns and present proposals with supporting evidence to authorized users for approval

**Synthesis & Analysis**

- **FR-010**: System MUST generate weekly synthesis reports per persona showing themes, signal counts, and source distribution
- **FR-011**: System MUST generate cross-persona synthesis highlighting overlaps and unique needs
- **FR-012**: System MUST identify and highlight conflicting signals across sources
- **FR-013**: System MUST compute PMF scores per persona-pain-point combination based on signal sentiment and frequency, using a five-level categorical scale (Poor/Fair/Good/Strong/Excellent) with confidence levels that scale based on signal volume (1-100 signals)

**Product Priority Recommendations**

- **FR-014**: System MUST generate recommendations for product development priorities by analyzing gaps between persona needs and current capabilities
- **FR-015**: System MUST rank recommendations by estimated impact and required effort
- **FR-016**: System MUST identify open validation questions for each recommendation, specifying what assumptions need to be tested
- **FR-017a**: System MUST suggest appropriate research methods (customer interviews, surveys, usability tests) for each validation question
- **FR-017b**: System MUST specify target personas for validation activities based on the recommendation scope

**On-Demand Capabilities**

- **FR-018**: System MUST generate executive briefs on demand with customizable scope (persona, pain point, time range)
- **FR-019**: System MUST support "what-if" scenario analysis projecting impact of proposed changes
- **FR-020**: System MUST generate curated research briefs pulling relevant external research

**Operations**

- **FR-021**: System MUST run scheduled ingestion jobs at configurable intervals
- **FR-022**: System MUST run daily PMF score refreshes
- **FR-023**: System MUST run weekly synthesis generation automatically
- **FR-024**: System MUST update decision diaries when significant insights or decisions are recorded

**Privacy & Governance**

- **FR-025**: System MUST track explicit consent status per data source
- **FR-026**: System MUST anonymize data where feasible based on source requirements
- **FR-027**: System MUST enforce per-source visibility controls limiting who can see signals from specific sources
- **FR-028**: System MUST maintain complete data lineage for audit purposes
- **FR-029**: System MUST support consent revocation with appropriate data handling (anonymization or deletion)
- **FR-030**: System MUST automatically delete signal data after 6 months from ingestion date

### Key Entities

- **Signal**: A normalized piece of feedback or insight; contains source reference, timestamp, content summary, persona tags, pain-point tags, sentiment indicator, confidence score
- **Persona**: A customer archetype in the shared taxonomy; contains name, description, characteristics, associated pain points, creation date, version
- **Pain Point**: A problem or need in the shared taxonomy; contains name, description, severity, associated personas, creation date, version
- **Source**: A data origin with consent and visibility rules; contains type, name, consent status, visibility scope, last ingestion date, connection status, health metrics (uptime %, success rate, quality score, signal-to-noise ratio), write-back capability flag, required metadata fields, data format specification
- **Theme**: A synthesized pattern across multiple signals; contains description, supporting signals, confidence, persona association, time range
- **PMF Score**: A metric tracking product-market fit progress; contains persona reference, pain-point reference, score value (Poor/Fair/Good/Strong/Excellent), trend direction, confidence level, last updated
- **Recommendation**: A prioritized product development suggestion; contains description, functionality/solution details, impact estimate, effort estimate, supporting signals, ranking rationale, open validation questions
- **Validation Question**: An open question requiring research/testing; contains question text, priority level, suggested research method (interview/survey/test), target personas, linked recommendation, acceptance criteria for answer
- **Feedback Submission**: A correction or enrichment to a signal; contains signal reference, user identifier, timestamp, feedback type (correction/enrichment/flag), original values, new values, submission status (pending/completed/failed), retry count, source write-back status

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Product team members can submit feedback from any supported source type and see it normalized within 5 minutes
- **SC-002**: 90% of ingested signals are automatically tagged with at least one persona and one pain point without manual intervention
- **SC-003**: Weekly synthesis reports are generated automatically and delivered to stakeholders without manual trigger
- **SC-004**: Stakeholders report spending 50% less time manually aggregating feedback across sources
- **SC-005**: PMF scores update within 24 hours of new signal ingestion
- **SC-006**: 80% of product development recommendations are rated as "actionable" by product managers
- **SC-007**: Each recommendation includes at least one validation question with clear research method suggestion
- **SC-008**: Executive briefs can be generated on-demand within 2 minutes of request
- **SC-008**: All signals maintain complete data lineage from source to insight
- **SC-009**: System correctly enforces consent and visibility rules with zero unauthorized data exposure
- **SC-010**: Cross-source contradictions are surfaced in 100% of synthesis reports where they exist
- **SC-011**: Data sources maintain 95% connection uptime and administrators are alerted within 2 hours of source failures
- **SC-012**: Signal quality scores improve by 15% over 3 months when feedback loops with sources are active
- **SC-013**: 90% of feedback corrections successfully write back to source systems within 10 minutes when sources support write operations

## Clarifications

### Session 2026-02-05

- Q: How should sensitive customer data be protected in terms of encryption and retention? → A: Minimal encryption, 6-month retention with automatic deletion
- Q: What scale and methodology should be used for PMF scoring? → A: Five-level categorical (Poor/Fair/Good/Strong/Excellent) based on signal patterns
- Q: What architecture pattern should handle processing workload? → A: Asynchronous batch processing with scheduled jobs (hourly ingestion, daily scoring, weekly synthesis)
- Q: What threshold determines "sufficient" signal volume for PMF scores? → A: No minimum, but confidence calculation scales with volume (1-100 signals)
- Q: How should new personas/pain points be added to the taxonomy? → A: Human-in-the-loop: AI proposes, human approves before integration

## Assumptions

- The company has existing data in the specified source systems that can be accessed programmatically or via structured export
- Stakeholders have agreed on an initial persona and pain-point taxonomy (can evolve over time)
- There is organizational alignment on what constitutes a "positive" vs "negative" signal for PMF scoring
- Consent tracking for existing data sources has been established or will be established as part of initial setup
- Users have appropriate permissions/roles defined for accessing different sensitivity levels of data
- Signal data is retained for 6 months maximum then automatically deleted to minimize storage costs and privacy exposure
- System uses asynchronous batch processing architecture; insights are not real-time but delivered through scheduled job runs
- Data sources are owned and maintained by teams that can provide necessary documentation, credentials, and support for integration
- Source systems have reasonable uptime SLAs (at least 95%) and provide stable data access methods
- Data sources provide sufficient metadata quality for at least 60% of their data points to meet minimum ingestion thresholds
- Write-back permissions to source systems can be obtained for authorized users when feedback submission is required
- Source system administrators will respond to schema change notifications within 48 hours
