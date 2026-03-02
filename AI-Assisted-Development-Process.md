# AI-Assisted Software Development Process

A practical guide for developers getting started with AI-assisted development workflows.

---

## Overview

This document describes a structured process for using AI tools effectively throughout the software development lifecycle. Whether you're building a new application from scratch or adding features to an existing codebase, following a deliberate process helps you get better results from AI collaboration and maintain control over your project's direction.

---

## Phase 1: Planning & Requirements

The process begins *before* you ever open an AI chat. Clear written planning is the foundation of effective AI-assisted development.

### 1.1 Create a Planning Document

Start by creating a markdown planning file for your project or feature. Do this regardless of scope. New applications and single-feature additions both benefit from written plans.

**What to include:**

- **What you want to build.** A clear description of the feature or application in plain language.
- **Attributes and behavior.** How it should work from the user's perspective. Describe inputs, outputs, expected behavior, and edge cases.
- **Constraints.** Any technical requirements, compatibility needs, performance targets, or limitations.

### 1.2 Architecture Document (New Applications)

For new applications, create a separate architecture document that covers:

- Technology stack choices and rationale
- High-level component/module structure
- Data models and storage approach
- API design or integration points
- Deployment considerations

Keeping architecture separate from feature planning helps you reference it across multiple features without cluttering individual plans.

### 1.3 AI Review of the Planning Document

Once you have a solid draft, reference the document in your prompt and ask the AI to review it. Specifically ask it to:

- Identify gaps or ambiguities in your requirements
- Suggest details you may have overlooked
- Point out potential technical challenges
- Recommend clarifications that would help it (or any developer) implement the plan more effectively

This review step often surfaces things you assumed but didn't write down, which are exactly the details that lead to misaligned implementations.

### 1.4 Iterative Refinement

The initial AI review is just the beginning. This is a collaborative, back-and-forth process where you steer the direction and the AI handles the document updates.

**How the workflow operates:**

You create the initial seed document to get things started. From that point forward, the AI updates the document as you discuss changes together through conversation. Your role shifts from writer to director. You provide answers, decisions, and direction through dialog, and the AI incorporates those into the document in real time.

**The refinement loop:**

1. **Discuss and direct.** Respond to the AI's feedback through conversation. Provide answers, clarify intent, and make decisions. The AI updates the document to reflect what's been agreed upon.
2. **Ask for another round of feedback.** After changes are incorporated, ask the AI if it has further feedback on the updates or if it believes anything else is missing. This keeps the cycle moving forward.
3. **Repeat until both sides are satisfied.** Continue the cycle until you and the AI agree that there is enough detail to begin development. You'll know you're done when the AI's feedback shifts from "this is missing" to minor polish suggestions.

**Making the refinement loop more effective:**

- **Periodic checkpoints.** Every few iterations (or whenever a significant section gets reworked), pause and read the full document back yourself. When the AI is making the edits, it's easy for subtle drift to occur. A phrase gets reworded, a priority shifts, or something you cared about gets softened. A quick read-through confirms the document still reflects what's in your head.
- **Flip the prompt.** Instead of always asking "is anything missing?", occasionally change the angle. Try prompts like:
  - *"If you were a developer tasked with building this, what questions would you still have?"*
  - *"What assumptions are we making that aren't written down?"*
  - *"Try to poke holes in this plan. What could go wrong or what have we overlooked?"*

  Each of these surfaces a different class of gaps than a straightforward review request.

The goal is a document that is thorough enough that a developer (human or AI) could implement it without needing to make assumptions.

### 1.5 Spin Off Detailed Sub-Documents

During the iterative review process, certain areas of functionality will grow complex enough to warrant their own dedicated document. When this happens, break that section out into a separate page.

- Each sub-document should cover a specific area of functionality in full detail.
- The main planning document should reference these sub-documents so the full scope remains navigable from one place.
- This keeps individual documents focused and manageable rather than letting the main plan balloon into an unwieldy monolith.

By the end of this phase, you should have a collection of fully documented planning artifacts: a main planning document, an architecture document (for new applications), and any number of detailed feature or functionality documents.

---

## Phase 2: Technology Stack Validation

Before writing any code, validate that your technology choices are sound. This phase bridges planning and implementation.

### 2.1 Discuss and Verify the Stack

Have a focused conversation with the AI about your technology stack. Review each choice against the architecture and expected use to confirm it's the right fit. Questions to work through:

- Does this stack align with the application's architecture and requirements?
- Are there performance, scalability, or compatibility concerns given what we've documented?
- Are there better alternatives we haven't considered?

### 2.2 Security and Version Verification

Before locking in the stack, verify that the specific versions you plan to use are secure and current:

- Check for known CVEs (Common Vulnerabilities and Exposures) against each dependency and its version.
- Ensure you're not adopting versions with outstanding security issues.
- Confirm that the chosen versions are actively maintained and will receive security patches for the expected life of the project.

Document the verified stack and versions so you have a clear record of what was validated and when.

---

## Phase 3: Development Phase Planning

With a fully documented plan and a validated technology stack, the next step is to break the development work into structured phases.

### 3.1 Context-Window-Aware Phase Breakdown

This is a critical step that many developers overlook when working with AI. LLMs have a finite context window, and when that window fills up, the model must compact its memory, which can cause it to lose important details mid-development.

To avoid this, break the development into phases that are small enough to fit comfortably within the LLM's context window. Each phase should be:

- **Self-contained.** It has a clear start and end point, with well-defined inputs and outputs.
- **Scoped to what the LLM can hold.** The phase's planning documents, relevant code, and conversation history should all fit within context without requiring compaction.
- **Sequenced logically.** Later phases can build on earlier ones, but each phase should be completable in a single focused session.

### 3.2 Validate Phase Order with the AI

Once you have a draft breakdown of phases, ask the AI to validate the logical sequencing. The AI should verify that each phase's dependencies and prerequisites are produced by an earlier phase. No phase should assume something exists that has not been built yet.

For example, if Phase 3 involves building an API layer that reads and writes data, then the data models, database schema, and migration scripts must already exist from a prior phase. If Phase 4 builds a UI that calls that API, then the API endpoints must be complete and testable before Phase 4 begins.

This sounds obvious, but in practice it's easy to accidentally sequence work in a way that creates circular dependencies or forces you to go back and retrofit something that should have been built earlier. Having the AI audit the phase order catches these issues before they become costly mid-development.

### 3.3 Phase Documentation

Document each phase with enough detail that you could start a fresh AI session, provide the phase document, and pick up cleanly. This includes:

- What is being built in this phase
- What prerequisites or outputs from prior phases it depends on
- Acceptance criteria, specifically how you will know the phase is done

### 3.4 Final Review of All Planning Artifacts

Before moving forward, read through the full set of documents yourself, including the architecture, feature documents, and phase plans, and verify that they still align with the original goal you have for the project. Over the course of iterative refinement, details evolve, and it's important to confirm that the accumulated changes haven't drifted away from your intent.

This is your last checkpoint before development begins. If something feels off, this is the cheapest time to correct it.

---

## Phase 4: Documentation (Pre-Development)

Before writing any code, have the AI produce official documentation for the feature or application being built. This is separate from the architecture and planning documents. It is the kind of documentation that developers or end users would reference for answers about how the application works.

### 4.1 Why Document Before Development

Writing documentation before implementation may seem counterintuitive, but it serves a powerful purpose. It forces you and the AI to articulate how the application or feature should behave from the perspective of someone using or maintaining it. If something is hard to explain clearly in documentation, it's often a sign that the design needs more thought.

This documentation also becomes a reference point during development. If the implementation doesn't match what the docs describe, either the code or the docs need to change.

### 4.2 What to Document

Ask the AI to produce documentation appropriate to the audience:

- **Developer documentation.** API references, data models, configuration options, how to extend or integrate with the application.
- **User documentation.** How to use the feature, expected workflows, inputs and outputs, common scenarios.

The scope depends on what you're building. A backend service might only need developer docs; a user-facing feature likely needs both.

### 4.3 Alignment Review

Once the documentation is drafted, review it against your planning and architecture documents to ensure everything is consistent. The planning docs describe *what* you intend to build and *how* it's structured; the official documentation describes *how it works* from the outside. These should tell the same story.

If you spot discrepancies, resolve them now. Update whichever document is wrong before development starts.

---

## Phase 5: Development Execution

This is where code gets written. Each development phase follows a structured cycle: fresh context, focused implementation, multi-layered review, and a clean merge.

### 5.1 Start Each Phase with a Clean Context

Clear the LLM's context window before beginning a new phase. Then direct the AI to review the phase document (and any associated code or reference documents) before writing any code. This ensures the LLM has a focused, uncluttered understanding of exactly what needs to be built, free from the accumulated conversation of prior phases.

This is why the phase documents (section 3.3) need to be self-contained. They are the handoff mechanism between sessions.

### 5.2 Implementation

With the phase document loaded, the AI begins development. Work through the phase collaboratively. The AI writes code, you review, provide feedback, and iterate until the phase's acceptance criteria are met.

### 5.3 Multi-LLM Code Review

Once a phase's implementation is complete, run a structured code review process using multiple LLMs. This is one of the most valuable practices in AI-assisted development. No single LLM catches everything, and different models have different strengths.

**The review cycle:**

1. **External LLM reviews (two different models).** Use two LLMs other than your development LLM. Submit the changes to the first LLM for code review. Feed its feedback back to the development LLM to address. Then submit to the second LLM for review and repeat the correction cycle.

2. **Development LLM self-review.** After the external reviews are addressed, clear the development LLM's context and ask it to perform a fresh code review on the cumulative changes (typically diffed against main or the current branch). A clean context forces it to look at the code with fresh eyes rather than carrying assumptions from the implementation session.

3. **Repeat across all three LLMs.** Continue until all three LLMs have reviewed the code and their feedback has been addressed.

4. **Your own review.** Perform a visual inspection of the code yourself. The LLMs catch a lot, but you understand the intent and the broader project context in ways they don't.

### 5.4 Code Review Instruction Files

To get the most out of multi-LLM reviews, create a dedicated code review instruction file (e.g., `CODEREVIEW.md`) that you reference only during review sessions. Unlike files such as `AGENTS.md` or `CLAUDE.md` that get loaded into every session and consume context, a review instruction file is loaded with purpose, only when it's needed, and discarded afterward.

**Consider role-specific review focus areas.** Since you're using three different LLMs for review, you can assign each one a different lens to ensure structured coverage rather than three overlapping opinions:

- **LLM 1, Correctness and Logic.** Does the code do what the spec says? Are there logic errors, off-by-one mistakes, unhandled edge cases, or incorrect assumptions?
- **LLM 2, Security and Error Handling.** Are inputs validated? Are there injection risks, improper error handling, exposed secrets, or missing authentication/authorization checks?
- **LLM 3 (Development LLM), Maintainability and Performance.** Is the code clean, readable, and well-structured? Are there performance bottlenecks, unnecessary complexity, or violations of the project's architectural patterns?

You can structure this as a single `CODEREVIEW.md` with sections per role, or as separate files. Either way, you only load the relevant instructions for each review pass. See the companion file `CODEREVIEW.md` for a ready-to-use implementation of this approach, including detailed checklists, prompt templates, and narrative instructions for each review role.

### 5.5 Pull Request and Automated Review

Once all three LLM reviews pass and your own inspection is complete, create a pull request.

Submit the PR to your third-party code review service (e.g., Bito.ai) for an additional automated review pass. For each issue the service identifies:

1. Feed the issue back to the development LLM to fix.
2. Push a new commit with the fix.
3. Trigger the automated review again.
4. Repeat until the service marks the PR as accepted.

### 5.6 When Things Go Sideways

Not every phase goes according to plan. When problems surface during development, the severity determines the response. The key principle across all scenarios is: **stop, document what went wrong, and fix the plan before fixing the code.**

**Minor course corrections.** The AI implements something that technically works but doesn't match the intent, such as a wrong pattern, an over-engineered solution, or a misunderstood requirement. Stop as soon as you notice the drift. Clarify the intent in conversation and have the AI redo the work. The critical habit here is catching these early. If something feels off, address it immediately rather than letting the AI build more code on top of a shaky foundation. The longer you wait, the more work has to be unwound.

**Phase-level failures.** You reach the end of a phase and realize the approach fundamentally doesn't work. Maybe the data model can't support a feature planned for a later phase, or a library can't handle the actual workload. Resist the urge to patch it in place. Instead:

1. Stop the phase.
2. Document what failed and why. Write it down before you start fixing anything.
3. Go back to the planning documents and identify where the assumption broke.
4. Update the plans to account for what you've learned.
5. Restart the phase with a clean context and the corrected plan.

The sunk cost of the failed implementation is real, but building on top of a broken foundation usually costs more in the long run.

**Plan-level failures.** The rarest but most painful scenario. Development reveals that the architecture itself has a fundamental problem that can't be resolved by adjusting a single phase. When this happens, treat it as a mini re-planning effort:

1. Pause all development.
2. Document the failure thoroughly: what broke, why it broke, and what the original plan got wrong.
3. Revise the architecture and planning documents with the new understanding.
4. Re-validate the phase breakdown (section 3.2) since the sequencing may need to change.
5. Resume development from the earliest affected phase.

**Rollback checkpoints.** To make recovery from any of these scenarios less painful, ensure your version control is in a clean, mergeable state before each phase begins. If a phase goes badly, you can cleanly abandon it and restart without untangling partial work from good code. This is why the merge-and-branch workflow (section 5.7) matters. Each phase starts from a known-good baseline.

**Keep a failure log.** Whether the issue was minor or major, briefly document what went wrong and why. Over time, this log becomes a valuable resource. Patterns will emerge that help you refine your planning process and avoid repeating the same class of mistakes.

### 5.7 Merge and Advance

Once the PR is accepted, merge the code and create a new branch for the next phase. Repeat the entire cycle (5.1 through 5.6) for each subsequent phase.

### 5.8 Incremental Testing

Rather than deferring all testing to a dedicated phase at the end, include relevant unit tests and automated tests as part of each development phase. When a phase builds new functionality, the tests that verify that functionality should be written and passing before the phase is considered complete.

This practice has two key benefits. First, it catches regressions early. If a later phase breaks something built in an earlier one, the existing tests will surface the problem immediately rather than leaving it hidden until a final testing pass. Second, it gives the code review LLMs something concrete to validate against. Reviewers can check not only that the code looks correct but that the tests cover the right scenarios.

Dedicated testing phases later in the project can still be valuable for integration tests, end-to-end tests, and coverage gap analysis, but the core unit tests should grow alongside the code they cover.

### 5.9 What Phases Include

Development phases are not limited to application code. The full set of phases typically includes:

- **Application development.** The core feature or application code, with unit tests included in each phase.
- **Integration and end-to-end testing.** Broader test coverage that exercises the interaction between components built across multiple phases.
- **Documentation improvements.** Updates to the official documentation (Phase 4) to reflect what was actually built, including any deviations from the original plan.
- **Polish and final tweaks.** The final phase is typically reserved for cleanup: refining documentation, improving code comments, addressing minor UI/UX issues, and ensuring everything is production-ready.

---

## Phase 6: Post-Project Retrospective

After all phases are complete and the final code is merged, conduct a structured review of the process itself. This is not about the code. It is about how the process worked and where it can be improved for the next project.

### 6.1 What to Review

- Which phases took significantly longer than expected, and was that a planning problem or an execution problem?
- Did any LLM code reviews consistently catch issues that should have been prevented earlier in the process?
- Were there moments where the planning documents were insufficient and the AI had to make assumptions?
- Did the phase sizing work well for the context window, or did compaction still occur?
- What would you do differently next time?

### 6.2 Update the Process

Use the findings from the retrospective to update this process document. The goal is continuous improvement. Each project should leave you with a slightly better version of the process than the one you started with.

---

## Context Priming Strategy

When starting a fresh AI session for a new phase, what you load into context matters as much as clearing the previous context. Having a consistent approach ensures the AI starts each session with the right information and nothing extra.

### Recommended Session Startup Checklist

Before asking the AI to begin work, load the following into context:

1. **The phase document.** This is the primary input. It describes what needs to be built and what the acceptance criteria are.
2. **The architecture document.** Provides the structural context so the AI understands where the phase's work fits within the broader application.
3. **The tech stack decisions.** Ensures the AI uses the correct libraries, versions, and patterns that were validated in Phase 2.
4. **Relevant code from prior phases.** If the current phase builds on earlier work, include the key files or modules it will interact with.
5. **The code review instruction file (if entering a review session).** Only loaded during reviews, not during implementation.

Avoid loading documents that aren't directly relevant to the current phase. Each unnecessary document consumes context that could be used for the actual work.

---

## Tips & Principles

- **Write it down first.** The AI can't read your mind. The more clearly you articulate what you want in writing, the better the output.
- **Treat the AI as a collaborator, not an oracle.** Use it to review, challenge, and expand your thinking, not just to generate code.
- **Fresh context is your friend.** Clear the context window between phases. A focused LLM produces better work than one carrying baggage from hours of prior conversation.
- **Use multiple LLMs for review.** No single model catches everything. Different models have different blind spots, and structured multi-model review produces significantly more thorough coverage.
- **Document before you build.** Writing official documentation before implementation catches design problems early and gives you a reference to test the implementation against.
- **Keep instruction files purpose-specific.** Avoid loading review instructions, style guides, or other reference material into sessions where it isn't needed. Load it when it's relevant and discard it when it's not.
- **Test as you go.** Include unit tests in each development phase rather than deferring them. Tests that grow alongside the code catch regressions early and give reviewers something concrete to validate.
- **Retrospect after every project.** The process itself is a living thing. Review what worked and what didn't, and update accordingly.

---

*This is a living document. It will be expanded as the process is refined.*
