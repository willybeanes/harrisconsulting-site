# Rolling Out Claude Enterprise-Wide: What Actually Works

*Published 2026 · AI & Automation · 8 min read*

---

There's a version of an enterprise AI rollout that looks great in a slide deck: you license the tool, send an announcement email, and watch productivity soar. That version doesn't exist.

The real version involves IT constraints you didn't anticipate, integration limitations that force creative workarounds, and the persistent challenge of getting people who are already busy to change how they work. I've been living in that real version for the past year — leading a company-wide Claude deployment while simultaneously serving as the Gainsight admin responsible for making it actually connect to the systems our team uses every day.

Here's what I learned.

---

## The Gap Between "We Have AI" and "AI Is Saving Us Hours"

Most organizations that license an AI tool land somewhere in the middle of a spectrum. On one end: the tool sits unused because nobody knows what to do with it. On the other: it's deeply woven into how work gets done and hours are measurably recovered each week.

The gap between those two states isn't about the AI itself. Claude is capable. The gap is almost entirely about integration, workflow design, and change management.

When we started, the most common use case was essentially a fancy search engine — people pasting text in, asking questions, getting answers. Useful, but not transformative. The transformation came when we stopped treating it as a standalone chat tool and started connecting it to the actual systems where work lives.

---

## Connecting Claude to the Stack: What's Possible Without a Full Engineering Team

Our core operational stack includes Gainsight, Notion, and Google Workspace. The goal was to get Claude working across all three — surfacing context from one system while taking action in another, without requiring a developer on every integration.

The good news: Claude's connector ecosystem (via MCP — Model Context Protocol) has matured significantly. The less good news: in an enterprise environment with IT governance and locked-down custom configurations, you're often working with what's available rather than what's theoretically possible.

A few things that worked well in practice:

**Notion as a knowledge layer.** We use Notion heavily for documentation, runbooks, and process guides. Connecting Claude to Notion turned it from a Q&A tool into something closer to an institutional memory — you can ask it questions and get answers grounded in actual company documentation rather than generic responses. For onboarding new team members and answering repetitive process questions, this alone justified the investment.

**Gainsight context in real conversations.** Gainsight holds our customer health data, renewal timelines, and account history. Getting Claude to surface relevant account context during a conversation — without requiring someone to manually pull a report first — changed how our CS team prepares for calls. The workflow went from "let me pull up Gainsight and find the account" to "give me a summary of where this account stands before I get on this call."

**Google Workspace for document workflows.** Drafting customer-facing documents, summarizing meeting notes, and generating follow-up emails from call transcripts are all tasks that used to take meaningful time. With Claude connected to Drive and working alongside Gmail, these became near-instant.

---

## The IT Constraint Reality

Here's something nobody writes about enough: enterprise AI deployments don't happen in a vacuum. They happen inside organizations with security policies, IT governance structures, and legitimate reasons for controlling what tools can connect to what systems.

We ran into limitations around custom MCP connectors — the kind that would let you build truly bespoke integrations. In our environment, those required IT approval and infrastructure that wasn't immediately available. So we worked within the connectors that were already sanctioned.

This is actually fine. The lesson isn't "fight IT to get what you want." The lesson is: **the available connectors, used well, are more powerful than custom integrations used poorly.** Most of the value we captured came from Google Drive, Notion, and the Claude web interface itself — not from anything exotic.

Design for the constraints you actually have, not the ideal environment you wish you had.

---

## Adoption: The Part That Takes Longer Than You Expect

You can build a perfect integration and still have people not use it. Adoption is its own project, separate from the technical work.

A few things that moved the needle for us:

**Show, don't tell.** Abstract explanations of what AI can do don't change behavior. A five-minute live demo of Claude drafting a renewal email from account notes — that changes behavior. Get in front of your team and show the specific tasks they do every day, done faster.

**Start with the people who are already curious.** Every team has early adopters who are willing to experiment. Equip those people first, let them become internal advocates, and let adoption spread through trusted peer recommendations rather than top-down mandates.

**Name the friction.** Part of our job was helping people articulate what was actually slowing them down. Sometimes they'd been living with a slow process so long they stopped noticing it. "How long does it take you to prep for a QBR?" is a better conversation starter than "wouldn't AI be useful?"

**Build prompts they can actually use.** One of the highest-leverage things you can do is create a library of ready-to-use prompts for your team's most common tasks. Not generic prompts — prompts that reference your actual products, customers, and processes. The barrier to starting drops significantly when someone doesn't have to figure out how to ask.

---

## What I'd Do Differently

If I were starting this rollout again, I'd do three things differently:

**Map the high-value workflows before touching any technology.** We did some of this, but not enough. The integrations that delivered the most value were the ones anchored to specific, recurring tasks with clear time costs. The ones that delivered less were the ones we built because they seemed interesting.

**Budget time for prompt engineering.** Writing a good prompt for a business workflow is a skill. It's not hard, but it takes iteration. We underestimated how long it would take to go from "Claude can help with this" to "here is the exact prompt our team uses for this task every time."

**Set a 90-day check-in cadence.** AI tools evolve fast. What's not possible at launch may be possible three months later. Build in a regular review cycle to reassess what's available and what your team's evolving needs look like.

---

## The Bottom Line

Enterprise AI is not a product you buy — it's a practice you build. The organizations getting the most out of tools like Claude aren't the ones with the biggest budgets or the most sophisticated IT infrastructure. They're the ones who did the workflow analysis, built the integrations deliberately, and invested in helping their people actually change how they work.

That work is operations work. It's the same discipline as any other system implementation — just with a more interesting tool at the center.

If you're early in this process and want to compare notes, reach out. I'm happy to talk through what worked, what didn't, and what I'd tackle differently.

---

*Will Harris is an independent consultant specializing in Salesforce, Gainsight, Looker, and enterprise AI deployment. You can reach him at [will@harrisconsulting.us](mailto:will@harrisconsulting.us).*
