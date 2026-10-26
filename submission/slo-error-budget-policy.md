# Stan's Robot Shop — SLO and Error-Budget Policy

**Status:** Proposed for the Capstone 2 project
**Service:** Stan's Robot Shop on Amazon EKS
**Review cadence:** Weekly and after every significant incident

## 1. Service-level objective

The availability objective is **99.5% successful requests over a rolling
30-day window** for the customer-facing Robot Shop web application.

A request is considered successful when it completes without a server-side
error. Requests rejected because of invalid client input should not count
as service failures. Health checks, synthetic monitoring traffic, and
internal requests should be excluded from the calculation where practical.

This is a proposed objective. Actual compliance must be evaluated from
measured request counts and failures; it must not be assumed from pod health
alone.

## 2. Measurement

Use request metrics from the application or an agreed monitoring source.
Calculate availability as:

Successful eligible requests / Total eligible requests * 100

Availability is met when the result is at least 99.5% during the rolling
30-day window. If reliable request-level metrics are unavailable, record
that limitation and do not claim verified SLO compliance.

## 3. Error budget

For a 99.5% availability objective, the error budget is 0.5% of eligible
requests in the measurement window.

For example, with 100,000 eligible requests, up to 500 failed requests
would consume the full budget.

The example is illustrative, not a measurement of this deployment.

## 4. Policy when the budget is consumed

- **More than 50% consumed:** Review the trend and investigate recurring
  errors, latency, resource pressure, and recent changes.
- **More than 75% consumed:** Prioritize reliability fixes and review risky
  deployments before proceeding.
- **100% consumed or SLO breached:** Pause non-essential risky changes,
  prioritize mitigation and recovery, and document the incident.
- Resume normal change risk after reliability has recovered and corrective
  actions have been agreed.

Urgent security fixes and actions needed to restore service should not be
blocked by this policy.

## 5. Alerting and response

Monitor request failures and service health. Alerts should link to the
relevant dashboard and runbook. The responder should acknowledge the alert,
assess customer impact, mitigate first, and investigate root cause after
service stability is restored.

An alert rule existing in Grafana does not prove that notifications reach
the intended channel. Delivery must be tested and recorded separately.

## 6. Ownership and review

The project operator reviews the objective and available measurements
weekly, after incidents, and before significant releases. Any change to
the objective must be documented with its rationale.

## 7. Current evidence and limitations

- Prometheus and Grafana are deployed and connected.
- A Robot Shop monitoring dashboard has been exported as JSON.
- Fluent Bit and Loki are deployed, and service log searches have been
  verified.
- The 99.5% objective is proposed; compliance has not yet been established.
- Request-level SLO measurement, notification delivery, and incident-drill
  evidence must be verified before claiming full completion.
