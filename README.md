# AI-Assisted Software Development

A structured process and companion tools for developers who use AI throughout the software development lifecycle.

## What's in This Repository

### AI-Assisted-Development-Process.md

The core process document. It walks through a complete workflow for AI-assisted development, from initial planning through implementation, review, and retrospective. The process is designed for developers who are new to working with AI tools and want a repeatable, disciplined approach rather than ad hoc prompting.

The document covers six phases: planning and requirements gathering, technology stack validation, development phase planning, pre-development documentation, development execution with multi-LLM code review, and post-project retrospective. It also includes a context priming strategy for starting fresh AI sessions and a collection of tips and principles.

### CODEREVIEW.md

A companion file used during AI-assisted code reviews. It contains structured, role-specific review instructions designed to be loaded into an LLM's context only when performing a code review, not during implementation.

The file defines three review roles, each with a detailed checklist and narrative instructions: correctness and logic, security and error handling, and maintainability and performance. It also includes a prompt template for initiating review sessions and guidance on how to provide code to the reviewing LLM.

## How to Use These Documents

Start with **AI-Assisted-Development-Process.md** to understand the full workflow. When you reach the code review stage of development (section 5.3 of the process document), reference **CODEREVIEW.md** for the specific review instructions.

Both documents are intended to be living artifacts. Update them as you refine your process across projects.
