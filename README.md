# AI-Powered Incident Analyzer
## Technology Operations Intern — Practical AI Assignment 1

## 1. Project Objective

The objective of this project is to help Technology Operations engineers investigate application incidents by analyzing logs, identifying repeated failures, assessing severity, and recommending practical troubleshooting actions.

This document describes my proposed developer-side incident diagnosis and troubleshooting methodology.

**Project status:** Documentation and diagnostic approach are prepared. The full application implementation and end-to-end validation remain pending.

## 2. Incident Diagnosis Workflow

When an application issue is reported, I would follow these steps.

### Step 1: Understand the Incident

- Identify the affected application, service, endpoint, and environment.
- Determine when the issue started and whether it is ongoing.
- Check whether users or business transactions are affected.
- Review available monitoring alerts and service health indicators.
- Check recent deployments, configuration changes, and infrastructure events.
- Establish an initial incident timeline.

### Step 2: Collect and Validate Logs

- Collect relevant application logs covering the incident time window.
- Verify timestamps, severity levels, service names, and request IDs.
- Check whether the logs are empty, incomplete, malformed, or inconsistent.
- Correlate entries across related services.
- Remove secrets and sensitive information before analysis.
- Preserve the original log entries for verification.

### Step 3: Identify Errors and Repeated Patterns

- Identify ERROR, CRITICAL, FATAL, and WARNING entries where supported by the log format.
- Group similar error messages to identify recurring failures.
- Look for API timeouts, database connection failures, authentication errors, and resource exhaustion.
- Correlate errors using timestamps, request IDs, and service names.
- Distinguish symptoms from confirmed root causes.

### Step 4: Assess Incident Severity

Evaluate the incident using:

- Business and user impact.
- Number of affected services or transactions.
- Duration and frequency of failures.
- Criticality of the affected functionality.
- Available monitoring and operational evidence.

Assign a preliminary severity of Critical, High, Medium, or Low, and document the reason.

Error counts alone are not sufficient to determine severity.

### Step 5: Identify Possible Root Causes

- Review the detected error patterns.
- Form hypotheses based on the available evidence.
- Identify alternative explanations.
- Determine which metrics, logs, traces, or system checks can validate each hypothesis.
- Clearly distinguish observed facts from assumptions.
- Avoid treating AI-generated explanations as confirmed root causes.

### Step 6: Recommend Troubleshooting Actions

- Begin with safe, non-destructive diagnostic checks.
- Investigate the most likely causes using supporting evidence.
- Make changes only when authorized and after considering the risks.
- Define how recovery will be verified.
- Escalate when the issue persists, the impact is significant, or specialist access is required.

### Step 7: Generate an Incident Summary

The final report should contain:

- Incident description.
- Affected service and time window.
- Severity and rationale.
- Key errors and repeated patterns.
- Supporting log evidence.
- Possible causes and their confidence or uncertainty.
- Recommended troubleshooting actions.
- Escalation guidance.
- Known limitations and outstanding checks.

## 3. Severity Classification

| Severity | Description | Recommended response |
|---|---|---|
| Critical | Major outage, widespread severe impact, or critical functionality unavailable | Follow the incident escalation process immediately |
| High | Significant degradation or repeated failures affecting an important service | Investigate promptly and involve the service owner when needed |
| Medium | Partial or intermittent failure with limited impact | Investigate, monitor, and track the incident |
| Low | Minor issue with no clear user impact | Document and monitor for recurrence |

These are preliminary guidelines. The organization's incident policy and service-level objectives take precedence.

## 4. Example Incident: Payment API Timeout

### Observed symptoms

Suppose application logs contain:

- Repeated payment API timeouts.
- Database connection failures.
- Connection-pool exhaustion.
- Increased request latency.

### Initial diagnosis

**Observed evidence:** Payment requests are timing out, and database connectivity errors are present.

**Possible root cause:** Database unavailability or connection-pool exhaustion may be contributing to payment API timeouts.

**Potential impact:** Payment requests may be delayed or fail. The actual customer impact must be confirmed.

**Preliminary severity:** High if payment functionality is significantly affected.

This diagnosis is a hypothesis, not a confirmed root cause.

### Troubleshooting procedure

1. Confirm whether payment requests are currently failing.
2. Check payment-service health, latency, and error rates.
3. Check database availability and database-side logs.
4. Inspect connection-pool utilization, active connections, and configured connection limits.
5. Review CPU, memory, disk, and network metrics.
6. Correlate payment and database logs using timestamps and request IDs.
7. Review recent deployments and configuration changes.
8. Check whether retries are increasing the load on the service.
9. Identify the underlying cause before proposing remediation.
10. Verify recovery using health checks, error rates, latency, and safe test transactions.

If the problem continues or requires specialist access, escalate to the relevant service owner or on-call engineer.

**Important:** Do not immediately restart services or increase database connection limits without understanding the cause and following the approved change process.

## 5. Troubleshooting Decision Guide

### Problem A: Logs cannot be loaded

- Verify that the file exists and is readable.
- Check file format, encoding, permissions, and size.
- Confirm that the input is not empty.
- Test with a known-good sample log.
- Provide a clear validation message when input is invalid.

### Problem B: Errors are not detected

- Inspect the original log format.
- Verify how severity levels are represented.
- Check capitalization, timestamps, and structured JSON fields.
- Review parser assumptions.
- Preserve unmatched entries for manual inspection.

### Problem C: Repeated errors are incorrectly grouped

- Compare the original error messages.
- Identify variable fields such as timestamps and request IDs.
- Normalize messages carefully before grouping.
- Verify that distinct failure types are not combined.
- Test both matching and non-matching examples.

### Problem D: Severity classification appears incorrect

- Reassess the affected functionality and business impact.
- Check the incident duration and scope.
- Review the evidence supporting the severity.
- Determine whether the classification relied too heavily on error counts.
- Escalate immediately if evidence suggests a major outage.

### Problem E: Root cause remains uncertain

- Record the observed symptoms.
- List plausible causes and supporting evidence.
- Identify the next diagnostic check that can distinguish between them.
- Review monitoring metrics, service dependencies, traces, and deployment history.
- Keep the incident marked as under investigation until sufficient evidence is available.

### Problem F: AI output is incorrect or unsupported

- Compare generated claims with the original logs.
- Identify unsupported statements and contradictory evidence.
- Validate the response structure.
- Avoid presenting hypotheses as facts.
- Check API availability, authentication, rate limits, and timeout handling if an AI service is integrated.
- Use a deterministic fallback only if it has been implemented and tested.

### Problem G: The incident report is incomplete

- Verify that severity and rationale are present.
- Confirm that key log evidence is included.
- Check whether recommended actions relate to the detected errors.
- Mark missing information explicitly.
- Confirm that the report matches the analysis presented to the user.

## 6. AI-Assisted Analysis Approach

For the completed AI-enabled application, I would separate analysis into two stages.

**Stage 1: Evidence extraction**

Extract relevant log entries, timestamps, severity levels, and repeated patterns using deterministic processing.

**Stage 2: AI-assisted interpretation**

Use an AI model to summarize the evidence, suggest possible causes, explain potential impact, and recommend diagnostic checks.

The output should include supporting evidence and clearly identify uncertainty. The AI model should assist the engineer, not make unverified operational changes.

AI integration remains pending until an actual model has been connected and tested.

## 7. Testing and Validation Plan

| Test scenario | Expected result |
|---|---|
| Healthy application logs | No unsupported error findings |
| Repeated API timeouts | Timeout pattern identified |
| Database connection failures | Database-related evidence highlighted |
| Critical or fatal errors | High-priority findings surfaced with rationale |
| Empty input | Clear validation message |
| Malformed log entries | Safe handling and disclosed limitations |
| Multiple error types | Separate patterns and relevant evidence |
| Unsupported AI claims | Claims flagged or excluded |
| Report generation | Report accurately reflects the analysis |
