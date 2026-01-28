# Take-Home: Sava Founding Engineer

## Context

Sava is building the operating system for trusts. Unlike legal software that simply drafts documents, we are the actual regulated trustee. This means we hold assets, manage financial truth, and execute the wishes of families.

A core workflow we handle is **Distributions**: taking unstructured requests from beneficiaries (e.g., "I need tuition money") and turning them into immutable financial records, all while ensuring the trust never runs out of money.

## The Mission

Build a **Trust Officer Dashboard** that acts as "Mission Control" for a specific trust.

You are building the interface for a Trust Officer who needs to:

1. Monitor the financial health of the trust (**The Ledger**) (like a bank account)
2. A workflow to review and process incoming demands for cash against the trust's rules (**The Request Queue**).

## What We're Looking For

Specifically, we want to see:

* **Architectural thinking:** How do you structure your code, manage state, and model data? We also want to hear how you'd evolve this system in a production environment with real scale and regulatory requirements.

* **Thoughtful UX decisions:** How do you make complex financial workflows feel simple and understandable? 

* **Opinions:** We want to see evidence that an experienced dev made deliberate choices. What did you keep simple and why? What did you intentionally leave out? If you used clever prompts to get deeper, we'd love to see them.

* **Initiative:** If you have time and ideas, add features you think would be particularly cool.

**Tests are optional.** We care more about your thinking and taste than test coverage.

## The Constraints (suggestion)

* **Stack Suggestion:** Next.js, TypeScript, Tailwind CSS. If you're far more proficient in or feel strongly about a different stack, feel free to use whatever stack you'd like. 
* **AI Integration (Required):** Use the OpenAI API to parse unstructured requests. Your project should read the API key from a `.env.local` file (e.g., `OPENAI_API_KEY`). Do not commit your API key. We will use our own key when reviewing. Please just stick to OpenAI for the sake of this assignment. 
* **Take-home window**: 72 hours.

## On AI Coding Tools

You're welcome and encouraged to use AI coding agents (Claude Code, Cursor etc) to speed you up for this assignment. We care about the quality of the output and your ability to make good decisions. Please mention your process in your video walkthrough, AI coding hacks you use that you're proud of and how you code review AI-generated code and architecture to ensure that we're building a strong foundation.

## Input Data & Rules

You are provided with two JSON files in the `data/` directory and a set of Trust Rules.

### 1. The Data

* **`data/ledger.json`:** A history of all past transactions.
* **`data/requests.json`:** A list of unstructured text messages from beneficiaries, each with a submission timestamp.

### 2. The Trust Policy (The Rules)

Assume the Trust adheres to the following rules which should be used or referenced while recommending a decision to the trustee:

| Category | Policy |
| :---- | :---- |
| **Education** | Fully covered for all beneficiaries. Includes tuition, required materials, and academic travel. |
| **Medical** | Fully covered for all beneficiaries. |
| **General Support** | Monthly allowance capped at $5,000 per beneficiary. |
| **Large Purchases** | Any single request over $20,000 requires "High Priority" review and additional documentation. |
| **Prohibited** | Distributions for speculative investments (crypto, startups, gambling), luxury vehicles, and non-essential luxury goods are not allowed. |

---

## Core Requirements

### 1. The Ledger View (The Source of Truth)

* Display a list/table of the historical transactions from `ledger.json`.
* **Crucial:** Display the **Current Balance** of the trust. This must be **calculated dynamically** from the ledger history (sum of CREDITs minus sum of DEBITs). Do not hardcode it.
* When a request is approved, the new transaction should appear in the ledger and the balance should update immediately.

*Why this matters:* We want to see how you handle derived state and financial integrity.

### 2. The Request Queue (The AI Workflow)

* Display the incoming requests from `requests.json`, sorted by submission date.
* When a request is selected, use the **OpenAI API** to parse the `raw_text` into structured data:
  * **Amount** (extracted or estimated from the text)
  * **Category** (e.g., Education, Medical, General Support, Other)
  * **Urgency** (e.g., High, Medium, Low—based on language and deadlines mentioned)
  * **Policy Notes** (any relevant flags, e.g., "Exceeds $20k threshold" or "Potentially prohibited")

* **Decision Support:** Display the AI-extracted data alongside the **Trust Policy** so the officer can make an informed choice. The officer should be able to review and override the AI's suggestions before taking action.

### 3. The Interaction (The State Change)

Provide an interface to **Approve** or **Deny** each request.

**If Approved:**

* Create a new transaction record in the ledger (with appropriate description, amount, date, and type: DEBIT).
* Mark the request as "Approved" and remove it from the pending queue.
* The **Current Balance** must update immediately to reflect the deduction.
* The approved request should remain visible somewhere (e.g., a "Processed" section or as part of the ledger entry) for audit purposes.

**If Denied:**

* Mark the request as "Denied" and remove it from the pending queue.
* The balance should not change.
* The denied request should remain visible somewhere with its denial status for audit purposes.

**Important:** Requests should never simply "disappear"—there should always be a record of what happened to each request and when.

---

## Bonus Considerations

These are not required, but strong candidates often address them:

* **Pending Exposure:** How would you show the trust officer the total amount across all pending requests? If 8 requests totaling $175k are pending and the trust has $180k available, that's important context.
* **Request Age:** Some requests have been waiting longer than others. How do you surface urgency beyond what the beneficiary claims?
* **Partial Approvals:** What if the officer wants to approve a request for a different amount than requested?
* **Audit Trail:** Who approved what, when, and why?

---

## Deliverables

1. **GitHub Repository** containing:

   * Your complete source code
   * A `.env.example` file showing required environment variables (e.g., `OPENAI_API_KEY=your_key_here`)
   * A `README.md` with:
     * Clear instructions on how to run the app
     * Brief explanation of your technical decisions
     * Any assumptions you made

2. **Short Video Walkthrough (E.g. Loom, Required):**

   * **Demo the App:** Show yourself processing at least 2-3 requests (mix of approvals and denials) and verify the balance updates correctly.
   * **Code Walkthrough:** Briefly explain your architecture, specifically how you manage the state of the ledger to ensure accuracy.
   * **Trade-offs:** Explain one shortcut you took due to time constraints and how you would improve it in production.

---

## Getting Started

1. Click **"Use this template"** to create your own repository
2. Clone your new repository locally
3. Set up a Next.js project with TypeScript and Tailwind CSS
4. Copy the data files from `data/` into your project as needed
5. Create a `.env.local` file with your OpenAI API key
6. Build your solution!

**Expected starting balance from ledger.json:** $1,484,800
