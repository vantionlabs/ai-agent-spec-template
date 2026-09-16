<p align="center">
  <a href="https://vantion.co">
    <img src="https://raw.githubusercontent.com/vantionlabs/.github/main/profile/banner.png" alt="Vantion Labs" width="100%" />
  </a>
</p>

<h1 align="center">AI agent spec template</h1>

<p align="center">
  <b>A fill-in spec for AI agents: process, tools, approvals, evals and rollback.</b><br />
  The sections we use before anyone writes a prompt, with guidance and a worked example.
</p>

<p align="center">
  <a href="https://github.com/vantionlabs/ai-agent-spec-template/generate"><img alt="Use this template" src="https://img.shields.io/badge/Use_this_template-2233f0?style=flat-square&logo=github&logoColor=white" /></a>
  <a href="ai-agent-spec.md"><img alt="format: Markdown" src="https://img.shields.io/badge/format:_Markdown-f4f4f6?style=flat-square&logo=markdown&logoColor=white" /></a>
  <a href="LICENSE"><img alt="MIT" src="https://img.shields.io/badge/licence-MIT-f4f4f6?style=flat-square" /></a>
  <a href="https://vantion.co"><img alt="Vantion Labs" src="https://img.shields.io/badge/by-Vantion_Labs-2233f0?style=flat-square" /></a>
</p>

---

A fill-in spec for AI agents: process, tools, approvals, evals and rollback.

We write a spec for every agent before anyone writes a prompt. This template gives you the sections we use, with guidance for each and a short worked example.

## Files

- [`ai-agent-spec.md`](./ai-agent-spec.md): AI agent spec (Markdown)

## Why write a spec before you build an agent

An AI agent takes actions in your systems on behalf of people. Before the build starts, the team needs a shared, written answer to a few questions: which process the agent runs, what it may touch, who approves its work, how you will know it is doing well and how you switch it off. A spec is where those answers live.

A spec keeps scope from drifting. It gives security and legal something concrete to review, and it becomes the reference when someone asks months later why the agent is allowed to do something. It also exposes gaps early. If nobody can say what the agent should do when a supplier invoice has no purchase order, that is a process question to settle before any code is written.

The download, `ai-agent-spec.md`, contains:

- Headings for all ten sections, each with a guidance comment explaining what to write.
- Tables for data sources, tools, approvals, release criteria, alerts, risks and changes.
- A shortened worked example for a supplier invoice matching agent, so you can see the level of detail to aim for.

## The sections of the template

| Section | What it answers |
| --- | --- |
| 1. Summary and ownership | What the agent does in one paragraph, who owns the outcome, who runs it and what is out of scope. |
| 2. Process | The trigger, the steps today, the steps with the agent and the known exceptions. |
| 3. Inputs and data | Which systems and documents it reads, how fresh they are and where personal data is involved. |
| 4. Tools and permissions | Every tool it can call, whether it reads or writes, its scope and whose identity it runs under. |
| 5. Approvals and escalation | Which actions need a person, who that person is, what they see and what happens when nobody responds. |
| 6. Outputs and definition of done | What the agent produces, in what format, and how someone checks it. |
| 7. Evals and success criteria | The test set, the release thresholds and the measures you track after launch. |
| 8. Monitoring and rollback | Logs, alerts, the kill switch and the way back to the manual process. |
| 9. Risks and open questions | Known failure modes and decisions still to be made. |
| 10. Change log | What changed, when, which test set version it passed and who approved it. |

The order matters. Sections 2 to 5 describe the job and its limits, and the business owner should agree them before sections 6 to 8 are written, because thresholds and alerts only make sense once the scope is fixed. Keep section 9 honest: an open question that is written down gets an owner.

## The full guide

The rest of the guide, with worked examples, lives on our site: [AI agent specification template: define the process, tools and approvals](https://vantion.co/developers/ai-agent-spec-template).

---

Made by [Vantion Labs](https://vantion.co). MIT licensed: use it, change it, ship it.
