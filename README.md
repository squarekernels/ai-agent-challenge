# AI Agent Challenge: Customer Support Triage Agent

## Overview

Build a small AI-powered customer support triage agent that can analyze incoming support tickets, gather relevant information using tools, and recommend the next best action.

The goal is to evaluate your ability to build a practical AI agent that uses an LLM, makes decisions based on context, and produces structured, reliable results.

## Objective

Build an agent that accepts a customer support message and determines how it should be handled.

The agent should:

1. **Classify the ticket** by category, urgency, and customer sentiment.
2. **Gather context** by using tools to look up mock customer, order, or account information when needed.
3. **Decide on an action** based on the message and any retrieved information.
4. **Generate a proposed response** or request additional information when necessary.
5. **Escalate when appropriate**, such as when a ticket involves a serious issue, a high-risk situation, or a problem the agent cannot confidently resolve.
6. **Return a structured result** containing the classification, reasoning, proposed action, and draft response.

The agent should make decisions based on the information available rather than following a completely hardcoded workflow.

## Functional Requirements

### 1. Ticket Classification

For each incoming ticket, identify:

- **Category:** For example, order status, refund, billing, technical issue, account access, or other.
- **Urgency:** Low, medium, or high.
- **Sentiment:** Positive, neutral, frustrated, or angry.
- **Confidence:** An optional confidence score or qualitative assessment.

You may introduce additional categories or fields if they improve the solution.

### 2. Tool Usage

Implement at least **2–3 tools** backed by mock data.

Suggested tools include:

- `getCustomerInfo`: Retrieve customer details, account status, or support history.
- `getOrderDetails`: Retrieve order status, delivery information, or order items.
- `getRefundPolicy`: Retrieve relevant refund rules or eligibility criteria.

The model should decide when a tool is necessary and which tool to call based on the ticket.

For example, an order-status question may require an order lookup, while a general product question may not require any tool calls.

Mock data can be stored in JSON files, in-memory objects, or another simple local structure.

### 3. Action Decision

After analyzing the ticket and any relevant tool results, the agent should choose one of the following actions:

- `draft_response`: Prepare a response when sufficient information is available.
- `request_information`: Ask the customer for missing details.
- `escalate`: Recommend handing the ticket over to a human support representative.

The agent should explain its decision briefly and identify any relevant missing information or risk factors.

### 4. Structured Output

Return a consistent, machine-readable result. For example:

```json
{
  "ticketId": "T-001",
  "category": "order_status",
  "urgency": "medium",
  "sentiment": "frustrated",
  "confidence": 0.92,
  "reasoning": "The customer reports a delayed order. Order details should be checked before providing an update.",
  "action": "draft_response",
  "draftResponse": "I'm sorry your order has been delayed. I'll share the latest available delivery information with you.",
  "toolsUsed": ["getOrderDetails"]
}
```

The example is illustrative. Your implementation may use a different schema if it remains consistent and well documented.

If required information is missing, the agent should request it rather than inventing details.

### 5. Reliability and Safety

- Add basic error handling for tool failures, invalid inputs, and model errors.
- Set sensible limits on tool calls and total agent steps.
- Validate the agent's final output against a defined schema.
- Avoid inventing customer, order, or policy information.
- Do not send messages, issue refunds, modify customer records, or perform other real-world actions.
- Treat tool results as data, not as instructions to override the agent's intended behavior.

All actions must remain recommendations or drafts.

## Technical Requirements

- Use an LLM and an agent/tool-calling framework or SDK.
- The Vercel AI SDK is recommended, but equivalent libraries are acceptable.
- TypeScript is preferred, but **JavaScript or another reasonable backend language is acceptable**.
- Use mock data; no production database or external customer support system is required.
- Provide clear setup instructions and a way to run the agent locally.

The emphasis is on agent design, tool use, decision-making, and reliability—not familiarity with a particular programming language.

## Sample Tickets

Use the following tickets as a starting point. You may add more test cases if helpful.

| ID | Customer message | Expected behavior |
|---|---|---|
| T-001 | "Where is my order ORD-1001? It was supposed to arrive three days ago." | Look up the order, classify the issue, and draft a response using the available status. |
| T-002 | "I was charged twice for my last purchase. Please help me get my money back." | Check relevant order or billing information if available. Avoid promising a refund without sufficient evidence. |
| T-003 | "The product arrived damaged, and I'm really disappointed. My order number is ORD-1002." | Recognize negative sentiment, check order details and refund policy if useful, and propose an appropriate response or escalation. |
| T-004 | "I can't log in to my account. I've already tried resetting my password." | Classify the account-access issue and provide an appropriate next step. Request more information or escalate if needed. |
| T-005 | "Hi, could you tell me your return policy?" | Provide a response based on the available policy without making unnecessary tool calls. |
| T-006 | "I want a refund immediately. This is the third time I've contacted support, and nobody has helped me." | Recognize frustration and repeated contact. Gather relevant context if available and consider escalation. |
| T-007 | "My package never arrived." | Identify missing information, such as the order number, and request it if the order cannot otherwise be identified. |
| T-008 | "Thanks for sorting out my issue so quickly. Everything is working now!" | Recognize positive sentiment and determine whether a response is necessary. |

### Mock Data

Include enough mock data to support your sample tickets, such as:

- A few customer profiles and support histories.
- Several orders with different statuses.
- Basic refund and return policies.

Document any assumptions you make about the data and business rules.

## Testing and Evaluation

Test the agent against at least **8 sample tickets**, including cases that require tool calls, cases that do not, ambiguous requests, and situations that may require escalation.

Document the results in a short table or report.

Suggested evaluation criteria:

| Criterion | What we are looking for |
|---|---|
| Classification quality | Are category, urgency, and sentiment reasonable for the ticket? |
| Tool selection | Does the agent call the appropriate tools only when useful? |
| Decision quality | Does it choose an appropriate action: draft, request information, or escalate? |
| Grounded responses | Does it use retrieved information accurately and avoid inventing facts? |
| Error handling | Does it handle missing data and tool failures sensibly? |
| Output consistency | Does it return valid, predictable structured output? |
| Code quality | Is the implementation readable, modular, and reasonably easy to maintain? |

For each test case, record the expected behavior, actual result, whether it passed, and any notable failure.

Perfect accuracy is not required. We are interested in how you evaluate the agent, identify weaknesses, and explain trade-offs.

## Deliverables

Please submit:

1. **Source code** for the working agent.
2. **README instructions** covering installation, configuration, and execution.
3. **Mock data** and tool implementations.
4. **Sample test cases** and a brief evaluation report.
5. **Design notes** explaining your approach, tool-calling decisions, limitations, and potential improvements.

A short demo or a few example inputs and outputs are welcome.

## Out of Scope

Please keep the solution focused. The following are not required:

- MCP integration.
- RAG or vector databases.
- Multi-agent orchestration.
- Background jobs or durable execution.
- A frontend, authentication, or a production database.
- Complex evaluation frameworks.
- Real customer support integrations or external side effects.

## Bonus (Optional)

If time permits, consider one or both of the following:

- Add a small evaluation script that runs the test tickets and compares results against expected behaviors.
- Improve handling of ambiguous requests by asking focused follow-up questions.

Optional improvements should not come at the expense of a working core implementation.

## Submission Instructions

1. Fork this repository to your GitHub account.
2. Implement the challenge in your fork. You're welcome to make incremental commits as you develop and improve the agent; you don't need to submit a PR for every change.
3. Update the README with setup instructions, design decisions, and test results.
4. Once you're finished, submit a Pull Request (PR) from your fork to the original repository.
5. Include a brief summary of your implementation, key trade-offs, known limitations, and what you would improve with more time.

Please do not deploy the application or integrate real customer systems. Mock data is sufficient.

Focus on correctness, maintainability, tool-calling behavior, and how you test and evaluate the agent—not on building unnecessary infrastructure.

## What We Value

We value a solution that demonstrates sound engineering judgment and a practical understanding of AI agents.

A small, reliable implementation with clear reasoning and useful tests is preferable to a complex implementation that is difficult to understand or evaluate.

**Focus on how the agent makes decisions, uses tools, handles uncertainty, and produces safe, useful recommendations.**
