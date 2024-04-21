# SLOs and SLIs : How do I go about it?

- I sit down with stakeholders, come up with statements for the critical things they care about.
- We translate those critical paths into SLIs, by identifying metrics that matter.
- Get priority and firm handshakes on adding the metrics and other code needed in place to measure these SLIs.
- We negotiate SLO targets with the team, so that once the SLIs are in place, we can create rules and alerts for them.
- With SLOs established, SLI's in service to those SLO's in place, metrics and alerts to track them, we then move on into discussing short-term and long-term error budgets. 

---
### SLO Statement
"The value for X remains as expected for Y% over Z time-window"

### Rationale
"X represents as a metric/attribute of key interest because of (specific reason)"

"Y was calculated based on (specific reason)"

"Z is the time-duration in consideration for X because of (specific reason)"

### SLI
The metric that expresses X is : (some valid metric)

This metric is located in : (dataset/monitoring-platform) and is sourced from (client/server)

### Alert
This metric triggers an alert if the value is over/under X by (some threshold value) for (some amount of time)

This alert is configured at : (location of the alert based on system)