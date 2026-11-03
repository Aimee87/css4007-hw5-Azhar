# What the finished helpdesk should look like

Build a terminal program. A website is not required. The dialogue below describes the requested user experience. It is illustrative, not a transcript of a program supplied in this repository. Your wording, commands and internal node names may differ.

The final program should accept a user identifier, conversation/thread identifier and service-status profile. It should accept several messages in the same conversation, print replies, show or save its execution trace, and save ticket drafts locally. Document the exact commands you implemented.

## 1. A simple request becomes a visible path (S01–S05)

User: I cannot connect to Wi-Fi.

Helpdesk: Here are the Wi-Fi instructions from the local guide: check Wi-Fi is enabled, select Campus-Demo, and try reconnecting after forgetting the saved network.

Alongside the reply, your Part 1 trace shows each node visited and the state changes. Keep the original message visible and show when category and reply were set. Predict the route before looking at that trace.

Try the other Part 1 messages. After adding the clarification node, a message mentioning both topics should produce a question such as: “Is the problem connecting to Wi-Fi, or recovering your account password?” This is a visible change in graph behaviour, not just a different final sentence from an unchanged help node.

## 2. The assistant uses results before drafting a ticket (S06)

User: Show me the Wi-Fi troubleshooting steps.

Helpdesk: [Instructions retrieved from GUIDE-WIFI.]

User: I enabled Wi-Fi and forgot the saved network, but Campus-Demo still will not connect. Check whether Wi-Fi is down. If it is operating, prepare a ticket about this problem.

Helpdesk: The demo status source reports Wi-Fi is operating. I prepared a local draft describing the failed connection and the steps you already tried. Nothing has been sent.

Show the actual calls and results from find_instructions, check_service_status and prepare_ticket across this interaction. The ticket should use the user's stated steps, not invent additional troubleshooting. Give its local filename so a reader can open it. Use data/ticket_template.json for its fields; generate your own ticket identifier.

## 3. Changing the data changes the action (S07, S08, S14)

Repeat the conditional ticket request with the wifi_outage snapshot.

Helpdesk: The demo status source reports a Wi-Fi outage. Your request said to prepare a ticket if the service was operating, so I have not prepared that ticket. The data does not give a repair time.

Now select status_unavailable.

Helpdesk: I could not determine the Wi-Fi status. I cannot confirm that it is operating. Would you like a ticket draft explaining that the status could not be checked?

These are different outcomes. Your assistant must use the selected snapshot instead of always making a ticket. For S14, look up account_recovery rather than Wi-Fi. Do not invent a repair time or claim a real university outage.

## 4. Missing details lead to a question (S09)

User: Prepare a support ticket.

Helpdesk: What problem are you having, and what have you already tried?

User: The issue is Wi-Fi. I enabled it and tried reconnecting, but it still fails.

Helpdesk: [A local draft using those details, with any service status clearly identified as known or not checked.]

The assistant must not fill missing details with invented facts. Preparing a draft and sending a ticket are different actions; this application only prepares drafts.

## 5. A conversation survives closing the program (S10, S11)

User: My issue is Wi-Fi. I enabled it and tried reconnecting, but it still fails.

Close the program completely. Open it again with the same user identifier, thread identifier and saved storage.

User: Which problem and steps did we discuss?

Helpdesk: You reported a Wi-Fi problem and said that enabling Wi-Fi and reconnecting did not solve it.

Show both commands or launches in your report. Do not paste the old conversation into the new input. Your saved conversation state must provide the context. S10 checks recall within one running conversation; S11 checks it after a process restart.

## 6. A preference survives a new conversation, but stays with its user (S12, S13)

In the first thread, demo-user-a says: Please remember that I prefer replies in Kazakh.

Close the program and open a new thread for demo-user-a. Ask which language was requested. The assistant should recall the saved preference and respond accordingly without receiving the old transcript. Its trace should identify user memory as the source. Do not hard-code this preference or load it from demo_users.json.

Then open a thread for demo-user-b and ask whether a preference or previous problem has been shared. The assistant should not know demo-user-a's information. It should say that no preference or previous problem has been recorded for this new user, or ask for the information.

## What a useful trace contains

Save one small log per scenario in outputs/. Include:

- Scenario identifier, selected snapshot, user identifier and thread identifier.
- Nodes visited, relevant state updates, model tool requests and tool results.
- Which information came from the reference file, conversation state or user memory.
- The final response, any local ticket filename, and whether the loop limit was reached.

Do not log passwords or API keys. Exact generated wording need not match these examples. Your explanations must refer to your own saved runs.

## Run all the supplied scenarios

Use data/scenarios.json. For every S01–S14, record the command or manual steps, saved log, what happened, and what you would change if the behaviour was wrong. Explain the original versus changed graph for S04/S05, and the different snapshots for S06/S07/S08.
