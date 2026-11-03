# Supplied data

All information is fictional and local. No real university accounts or services are involved.

| File | Use |
| --- | --- |
| helpdesk.json | Wi-Fi and password instructions, topic/service mapping and ticket rules |
| service_status.json | Four named snapshots: normal, wifi_outage, recovery_outage, status_unavailable |
| demo_users.json | Two invented identifiers; preferences must be learned from messages |
| ticket_template.json | Fields for a local ticket draft |
| scenarios.json | Fourteen demonstration sequences, including explicit process restarts |

Select the snapshot named by a scenario. A password topic uses the account_recovery service; Wi-Fi uses wifi. An unavailable status is unknown, not operational and not a confirmed outage.

For each scenario, reset conversation and user memory before its first event when reset_before is true. Do not reset between events. A restart_process event means actually terminate the program and launch a new process against the same storage. It does not mean call the graph again in the same process.

For S04 use the original Part 1 graph; for S05 use the changed graph. Other Part 1 cases may use the original graph. Parts 2–3 use the model-and-tools version.

The fixture user identifier and thread identifier are supplied by your application when starting a session. Do not let a model choose which user's stored data to load. A new thread has a separate conversation history; a saved user preference can be shared across that user's threads.

The dataset contains no completed run logs, implementation or marking key. Example interactions in SCENARIOS.md explain the requested behaviour. Generate your own results.
