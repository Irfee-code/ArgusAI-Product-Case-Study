# ArgusAI — Product Case Study

> Less searching. More deciding.

ArgusAI is an AI-powered first responder for production incidents. It helps engineers investigate incidents by automatically gathering relevant telemetry, analyzing available evidence, and presenting a probable root cause with supporting evidence and recommended next steps.

The product is designed around one core idea:

**Give an on-call engineer a useful starting point instead of a blank page.**

---

## The Problem

When an incident occurs, engineers often have to move between multiple tools to understand what happened.

An alert tells them **that something broke**, but not necessarily **why**.

The investigation can involve:

- Metrics
- Logs
- Alerts
- Monitoring dashboards
- Application context

This creates investigation overhead during an already high-pressure situation.

---

## The Product Opportunity

Instead of forcing engineers to manually search across multiple sources, ArgusAI brings the initial investigation into a single workflow.

```text
Incident Alert
      ↓
Evidence Collection
      ↓
Incident Context
      ↓
AI Analysis
      ↓
Probable Root Cause
      ↓
Recommended Actions

The goal is not to replace the engineer.

The goal is to reduce the time and effort required to reach a useful first hypothesis.

Target User
Primary User

Engineers responsible for investigating production incidents.

These users need to quickly understand:

What happened?
What evidence supports the explanation?
What should I investigate next?
How confident should I be in the analysis?

ArgusAI is designed to support the engineer during the initial investigation rather than automatically taking control of production systems.

User Journey
Incident occurs
      ↓
Engineer receives alert
      ↓
Engineer needs to investigate
      ↓
ArgusAI gathers relevant telemetry
      ↓
AI analyzes the incident context
      ↓
Engineer receives probable root cause
      ↓
Engineer reviews supporting evidence
      ↓
Engineer decides the next action

The product focuses on reducing the investigation burden between receiving an alert and forming a useful, evidence-backed hypothesis.

Product Solution

ArgusAI provides an incident intelligence workflow that combines:

Incident information
Metrics
Logs
AI-generated root-cause analysis
Supporting evidence
Recommended actions
Confidence
Real-time updates

The result is a single interface for the initial investigation.

Key Product Decisions
1. Evidence + RCA Instead of RCA Alone

An AI-generated answer is not enough for a production incident.

ArgusAI presents the supporting evidence alongside the probable root cause so the engineer can verify the conclusion.

This helps users:

Understand why the RCA was generated
Verify the evidence
Identify incorrect reasoning
Decide whether a recommendation is safe to follow
2. Recommendations Instead of Automatic Remediation

ArgusAI deliberately does not automatically change production systems.

The current product focuses on:

Detect
  ↓
Investigate
  ↓
Explain
  ↓
Recommend

rather than:

Detect
  ↓
Automatically Change Production

This keeps the engineer responsible for the final decision while trust in the system is established.

3. Confidence as a Trust Signal

The system exposes a confidence score alongside the RCA.

The intention is not to treat confidence as absolute truth, but to give the engineer an additional signal when evaluating the AI's analysis.

Confidence should eventually be validated against actual RCA accuracy.

4. Real-Time Incident Delivery

Instead of requiring engineers to manually refresh the dashboard, incident information and AI analysis are pushed to the interface in real time.

This supports the time-sensitive nature of incident response.

Product Experience

The dashboard brings the investigation into one place.

Incident

Shows the active incident and its context.

System Metrics

Provides relevant telemetry such as:

CPU usage
Memory usage
Request rate
Error count
Latency
Evidence

Provides supporting log and metric evidence used during analysis.

AI RCA

Displays:

Probable root cause
Supporting evidence
Recommended actions
Confidence

The engineer can then use the output as a starting point for further investigation.

Success Metrics

The prototype does not claim measured product results.

The following metrics define how the product should be evaluated after user testing and real incident data are available.

North Star Metric
Time to Confirmed Root Cause

The time from an incident alert to a root cause that the engineer confirms as correct.

Speed Metrics
Time to First Useful Hypothesis

Time from incident detection to the first useful AI-generated hypothesis.

Time to Acknowledgement

Time between incident detection and engineer acknowledgement.

Quality Metrics
RCA Correctness / Helpfulness

Percentage of AI-generated RCAs rated correct or helpful by engineers.

Trust Metrics
Evidence Engagement

Percentage of incidents where engineers open or inspect supporting evidence.

Confidence Calibration

Relationship between AI confidence and actual RCA correctness.

Adoption Metrics
RCA View Rate

Percentage of incidents where the generated RCA is viewed by an engineer.

Guardrail Metrics

The product should also monitor:

Incorrect-RCA rate
Analysis latency
Cost per analysis
False-confidence rate

These metrics help ensure that improvements in speed do not come at the expense of reliability or trust.

Risks & Limitations
1. Wrong but Confident RCA

The highest-risk failure mode is an incorrect RCA presented with high confidence.

Mitigation
Show supporting evidence
Keep human review in the loop
Measure RCA correctness
Validate confidence against real outcomes
2. Uncalibrated Confidence

The confidence score may not accurately represent real-world correctness.

Mitigation

Compare confidence scores with confirmed incident outcomes and calibrate the model over time.

3. Sensitive Data in Logs

Production logs may contain sensitive information.

Mitigation

Introduce evidence redaction and evaluate self-hosted model options for sensitive environments.

4. Limited Context

The prototype does not yet incorporate every possible source of incident context, such as deployment history and distributed traces.

Mitigation

Add deployment/change correlation and distributed tracing.

5. Lack of User Validation

The current prototype has not yet been validated through real user interviews or production-scale incident data.

Next Step

Conduct interviews with engineers involved in incident response and use their feedback to validate the workflow and prioritize improvements.

6. Alert Storms

Large numbers of related alerts can increase investigation noise and AI analysis cost.

Mitigation

Introduce related-alert grouping and incident correlation.

Roadmap
NOW
RCA Feedback

Allow engineers to rate whether an RCA was useful or correct.

Evidence Redaction

Protect sensitive information before evidence is passed to the AI system.

User Interviews

Validate the problem, workflow and usefulness with engineers who handle production incidents.

NEXT
Deployment / Change Correlation

Connect incidents with recent deployments and infrastructure changes.

Distributed Traces

Add trace information to improve incident context.

Related Alert Grouping

Group multiple related alerts into a single incident.

LATER
Similar Past Incidents

Retrieve previous incidents with similar symptoms and context.

Slack / PagerDuty Integration

Bring incident intelligence into existing incident-response workflows.

Automated Postmortem Drafts

Use incident evidence and RCA information to generate initial postmortem drafts.

Approval-Gated Remediation

Explore controlled automation where remediation requires explicit human approval.

What the Prototype Demonstrates
Product
Problem framing
User understanding
Product solution design
User journey mapping
Product decisions
Trade-off analysis
Success metrics
Risk analysis
Roadmapping
Engineering
Spring Boot backend
FastAPI service
React dashboard
Prometheus metrics
Loki logs
Alertmanager alerting
LLM integration
WebSocket-based real-time updates
Technical Architecture
                    ┌─────────────────────┐
                    │    Spring Boot      │
                    │     Application     │
                    └──────────┬──────────┘
                               │
                         Metrics + Logs
                               │
              ┌────────────────┴────────────────┐
              │                                 │
      ┌───────▼────────┐              ┌─────────▼───────┐
      │   Prometheus   │              │      Loki       │
      │    Metrics     │              │      Logs       │
      └───────┬────────┘              └─────────┬───────┘
              │                                 │
              └────────────────┬────────────────┘
                               │
                       ┌───────▼────────┐
                       │  Alertmanager  │
                       │     Alerts     │
                       └───────┬────────┘
                               │
                               ▼
                     ┌──────────────────┐
                     │     FastAPI      │
                     │  Incident Agent  │
                     └────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │   LLM Analysis    │
                    │   Structured RCA  │
                    └─────────┬─────────┘
                              │
                         WebSocket
                              │
                    ┌─────────▼─────────┐
                    │  React Dashboard  │
                    │ Incident + RCA    │
                    └───────────────────┘
Product Case Study

The full case study presentation covers the complete product journey, including:

Problem discovery
Problem framing
User journey
Product solution
Product decisions
Trade-offs
Success metrics
Risks
Roadmap
📄 Full Case Study

View the ArgusAI Product Case Study

Product Demo
🎥 Demo Video

Watch the ArgusAI Product Demo

The demo shows the complete incident flow from triggering an incident through monitoring, alerting, AI analysis and the final React dashboard.

Engineering Repository

The complete technical implementation is maintained separately.

💻 ArgusAI Engineering Repository

View the Engineering Repository

The engineering repository contains:

Spring Boot application
FastAPI incident-analysis service
React dashboard
Prometheus
Loki
Alertmanager
LLM integration
WebSocket-based real-time updates
Project Status

ArgusAI is currently a working prototype.

The prototype demonstrates the end-to-end incident intelligence workflow, while the product case study identifies the validation, measurement and trust-building work required before treating the solution as a production-ready product.

About

ArgusAI was built as a product + engineering project exploring how AI can reduce the investigation burden during production incidents while keeping engineers in control.

Less searching. More deciding.
