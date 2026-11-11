# HW5 — My LangGraph helpdesk

## Part 1 — How the graph works

### My graph and state
### First graph

START

  |

read_request

  |
  
  +--> wifi_help ------> END
  
  |
  
  +--> password_help --> END
  
  |
  
  +--> other_help -----> END

### State

message  - user message

category - request category

reply    - bot response

1) read_request reads the message and sets the category.

wifi_help creates a Wi‑Fi response.

password_help creates a password response.

other_help asks a question.

2) A node changes the state. A conditional edge chooses the next node.

3) The next node receives the current graph state.

4) START = beginning of the graph.

END = end of the graph.

Compile = make the graph ready to run.

### Predictions before running

| Message | Predicted route | Actual route | What I learned |
| --- | --- | --- | --- |
| I cannot connect to Wi-Fi. |START → read_request → wifi_help → END|START → read_request → wifi_help → END|The graph correctly found a Wi‑Fi problem.|
| I forgot my PASSWORD. |START → read_request → password_help → END|START → read_request → password_help → END|The graph correctly found a password problem.|
| My laptop screen is broken. |START → read_request → other_help → END|START → read_request → other_help → END|The graph sent an unknown problem to other_help.|
| My Wi-Fi password does not work. |START → read_request → wifi_help → END|START → read_request → wifi_help → END|The graph selected Wi‑Fi because Wi‑Fi was checked first.|

### My change to the graph
### Updated graph
START


  |

read_request

  |
  
  +--> wifi_help ------> END
  
  |
  
  +--> password_help --> END
  
  |
  
  +--> other_help -----> END
  
  |
  
  +--> clarify_problem -> END
  
### New trace
Input: My Wi‑Fi password does not work.

Route: START → read_request → clarify_problem → END

Before, messages containing both Wi‑Fi and password were sent to wifi_help. Now, messages containing both topics are sent to clarify_problem, which asks the user which problem should be solved first.

LangGraph controls the workflow and chooses which node runs next.

My functions decide the category of the request and create the response.

## Part 2 — Model and tools
### Model / Tool Loop

User

  |
  

Model

  |

Tool

  |

Tool Result

  |

Model

  |

Answer

1) The graph checks the model output. If the model requests a tool, the graph runs the tool. Otherwise, the graph returns the final answer.
2) The model requests the tool. The tool node executes the tool and returns the result.
3) The tool result is added to the state. The model can use it to generate the final answer.
4) Part 1 used fixed routing rules. Part 2 allows the model to choose which tool to use.

Provider: Google Gemini

Model: gemini-3.5-flash

### Direct Tool Calls
find_instructions_tool → password instructions

check_service_status_tool → operational

prepare_ticket_tool → ticket draft created

### User Request
1) Wi‑Fi still does not work after I followed the instructions.
2) Is it down? If it is working, help me prepare a ticket.

Tool 1: check_service_status_tool
Result: operational

Tool 2: prepare_ticket_tool
Result: ticket draft created

Final Answer:
The Wi‑Fi service is operational. A ticket draft was prepared.

### Loop Limit
A loop limit was not implemented in the current prototype. In a complete version, a maximum number of tool calls (for example, 5) would be used to prevent infinite loops.

## Part 3 — Memory
### Conversation Memory
Conversation memory stores the current conversation, previous messages, and steps already tried. I used SqliteSaver and saved the conversation state in memory.db.

### User Memory
User memory stores user preferences. In my implementation, the user's preferred language is saved in user_memory.json. The saved preference remains available in future conversations for the same user.

thread_id identifies a specific conversation. One user can have multiple conversations, and each conversation has its own history. 

user_id identifies the user and stores preferences that can be used across different conversations.

| Situation | Command used | Log file | What happened |
| --- | --- | --- | --- |
| Refer to an earlier message |get_state(thread-1)|memory.db|Previous conversation state was loaded from memory.|
| Close, restart and resume |SqliteSaver + memory.db|memory.db|Conversation state remained available after restart.|
| Same user, new conversation |user_memory.json + thread_id|user_memory.json|Language preference remained available, while the conversation was a new thread.|
| Different user |user_memory.get("demo-user-b")|user_memory.json|No saved preference was found for the new user.|

Wi‑Fi instructions are retrieval because they are read from helpdesk.json.

Language preference is memory because it was saved from a previous conversation.

## Part 4 — Running and checking my work

TODO: point to the README setup steps, package versions, memory-reset instructions, tests and GitHub Actions workflow. Explain which mistake each test catches and which runs use a fake model.

## Choices and limitations

TODO: explain the storage and libraries I chose, why they fit this task, and what still does not work reliably.

## Supplied scenarios — my actual results

| Scenario | Command or manual steps | Log file | What happened / what I changed |
| --- | --- | --- | --- |
| S01 — Simple Wi-Fi route | | | |
| S02 — Case-insensitive password route | | | |
| S03 — Unsupported request | | | |
| S04 — Two topics before the graph change | | | |
| S05 — Two topics after adding clarification | | | |
| S06 — Instructions then local ticket | | | |
| S07 — The same request during a Wi-Fi outage | | | |
| S08 — Status source unavailable | | | |
| S09 — Missing ticket details | | | |
| S10 — Use the current conversation | | | |
| S11 — Persist and recall after restart | | | |
| S12 — Preference across threads without replay | | | |
| S13 — Do not share memory between users | | | |
| S14 — Password recovery outage | | | |
