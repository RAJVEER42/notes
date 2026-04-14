# Product Management - Complete Exam Notes

---

## SESSION 1: Introduction to Product Management (PM 101)

### What is a Product Manager?

A Product Manager (PM) is the person responsible for deciding **what** to build, **why** to build it, and ensuring it delivers **value** to users AND the business. They sit at the intersection of **Business + Technology + User Experience**.

> Think of a PM as the "CEO of the product" — but without direct authority. You influence, not command.

### PM vs PO vs PMM

| Role | Focus | Key Output |
|------|-------|------------|
| **Product Manager (PM)** | Strategy, vision, what to build & why | Roadmap, PRD, strategy docs |
| **Product Owner (PO)** | Execution, backlog management, sprint-level decisions | User stories, acceptance criteria, sprint backlog |
| **Product Marketing Manager (PMM)** | Go-to-market, positioning, messaging, launch | Positioning docs, launch plans, sales enablement |

- **PM** = decides the direction (strategic)
- **PO** = manages the delivery (tactical, often Scrum-specific)
- **PMM** = takes it to market (external-facing)

### The Product Triad (Core Team)

```
        PM (What & Why)
       /              \
      /                \
Design (UX/UI)  ----  Engineering (How)
```

- **PM** owns the problem space and priorities
- **Design** owns the user experience
- **Engineering** owns the technical solution
- They collaborate as equals — no one "bosses" the others

### Outputs vs Outcomes

| Outputs | Outcomes |
|---------|----------|
| Features shipped, lines of code, screens designed | User behavior change, revenue growth, retention improvement |
| "We launched a search feature" | "Search reduced time-to-find by 40%" |
| Easy to measure | Harder to measure, but what actually matters |

**Key exam point:** Good PMs focus on **outcomes** (impact), not **outputs** (activity). Shipping a feature is meaningless if it doesn't change behavior.

### Product Lifecycle Overview

```
Intro --> Growth --> Maturity --> Decline
```

| Stage | Goal | PM Focus |
|-------|------|----------|
| **Introduction** | Find product-market fit | Discovery, MVP, user interviews |
| **Growth** | Scale users & revenue | Optimization, growth loops, hiring |
| **Maturity** | Maximize profit, defend position | Efficiency, new segments, upsell |
| **Decline** | Extract value or pivot | Sunset plan, migration, cost reduction |

### Artifacts as Decision Tools

PM artifacts are NOT just documents. They are **decision-making tools**:
- **PRD** = aligns team on what we're building and why
- **Roadmap** = communicates strategic bets and sequencing
- **Metrics tree** = shows how daily work connects to business goals
- **Decision memo** = makes trade-offs explicit and reviewable

---

## SESSION 2: Product Thinking & Problem Framing

### Problem vs Solution

| Problem Space | Solution Space |
|---------------|----------------|
| "Users can't find relevant products quickly" | "Add an AI-powered search bar" |
| Understanding **what** the user struggles with | Deciding **how** to fix it |

**Golden Rule:** Fall in love with the **problem**, not the solution. If you jump to solutions too fast, you might solve the wrong problem entirely.

### JTBD (Jobs To Be Done) - Lite

Users don't buy products — they **hire** them to do a job.

**Format:** "When I [situation], I want to [motivation], so I can [expected outcome]."

**Example:** "When I'm commuting to work, I want to listen to news summaries, so I can stay informed without reading."

- Focuses on the **user's goal**, not your feature
- Ignores demographics — focuses on the **job/context**

### Outcome Statements

Rewrite features as measurable outcomes:

| Bad (Feature) | Good (Outcome) |
|---------------|----------------|
| "Add dark mode" | "Reduce eye strain complaints by 30% in 90 days" |
| "Build notification system" | "Increase 7-day retention from 40% to 55%" |

**Formula:** [Direction] + [Metric] + [From X] + [To Y] + [Timeframe]

### Constraints & Non-Goals

- **Constraints** = things you MUST work within (budget, timeline, regulations, tech stack)
- **Non-goals** = things you explicitly choose NOT to do (prevents scope creep)

**Example non-goal:** "We will NOT support offline mode in V1."

Writing non-goals is just as important as writing goals — it prevents scope creep and aligns expectations.

### Writing an Opportunity Brief

An opportunity brief frames the problem before jumping to solutions:

1. **Target segment** — Who has this problem?
2. **Problem statement** — What's the pain? (specific, evidence-based)
3. **Why now?** — Why solve this today vs 6 months ago?
4. **Constraints** — Budget, time, technical, legal
5. **Non-goals** — What we won't do
6. **Evidence** — Data/quotes that prove the problem exists

### Avoiding Bias

| Bias | What It Is | How to Avoid |
|------|-----------|--------------|
| **Confirmation bias** | Seeking data that confirms your hypothesis | Actively seek disconfirming evidence |
| **Survivorship bias** | Only looking at successful users | Also study churned/failed users |
| **Anchoring** | First data point skews all subsequent thinking | Get multiple independent estimates |
| **Sunk cost fallacy** | Continuing because you already invested | Evaluate based on future value only |

---

## SESSION 3: Customers, Segments & Market Sizing

### Segmentation

Dividing your total market into distinct groups based on:
- **Demographics** — age, income, location
- **Behavioral** — usage frequency, spending patterns (most useful for PMs)
- **Psychographic** — values, attitudes, lifestyle
- **Firmographic** (B2B) — company size, industry, revenue

**Best practice:** Segment by **behavior** and **needs**, not just demographics.

### Personas vs ICP

| Persona | ICP (Ideal Customer Profile) |
|---------|------------------------------|
| A fictional user archetype | The specific type of customer most likely to buy & stay |
| "Sarah, 28, UX designer in NYC" | "Mid-market SaaS companies, 50-200 employees, with a dedicated ops team" |
| Used for empathy & design decisions | Used for sales targeting & go-to-market |
| More common in B2C | More common in B2B |

### Adoption Barriers

Why users DON'T adopt your product:
1. **Value barrier** — "I don't see why I need this"
2. **Usability barrier** — "It's too hard to use"
3. **Risk barrier** — "What if it doesn't work / wastes my money?"
4. **Tradition barrier** — "I'm fine with my current way"
5. **Image barrier** — "This isn't for people like me"

### B2B: Buying vs Using

In B2B, the **buyer** and the **user** are often DIFFERENT people:
- **Buyer** = CTO, VP Engineering (cares about ROI, security, compliance)
- **User** = Developer, analyst (cares about ease of use, speed)
- **Champion** = internal advocate who pushes for your product
- **Blocker** = person who can kill the deal (legal, procurement, IT security)

You must satisfy ALL of them — not just the end user.

### Market Sizing: TAM / SAM / SOM

```
TAM (biggest circle) > SAM (medium) > SOM (smallest)
```

| Term | Meaning | Example (Online Tutoring App in India) |
|------|---------|----------------------------------------|
| **TAM** (Total Addressable Market) | Everyone who could theoretically use your product | All students in India needing tutoring = $50B |
| **SAM** (Serviceable Addressable Market) | The part of TAM you can realistically reach | English-medium K-12 students in urban India = $5B |
| **SOM** (Serviceable Obtainable Market) | What you can realistically capture in 2-3 years | 2% of SAM in first 2 years = $100M |

**Top-down approach:** Start from industry reports and narrow down.
**Bottom-up approach (preferred):** # of customers x average revenue per customer x frequency. More credible.

---

## SESSION 4: Competitive Landscape & Positioning

### Competitors vs Alternatives

| Competitors | Alternatives |
|-------------|-------------|
| Direct products solving the same problem | ANY other way the user solves the problem today |
| Notion vs Coda | Notion vs pen & paper, spreadsheets, email |

**Always map alternatives** — your biggest "competitor" is often doing nothing or using a hacky workaround.

### Differentiation

How you are meaningfully different from competitors:
- **Feature-based** — "We have X that others don't" (weakest, easily copied)
- **Experience-based** — "Our UX is 10x simpler" (medium)
- **Ecosystem-based** — "We integrate with your entire stack" (strong)
- **Network-based** — "More users = more value" (strongest, hardest to replicate)

### Switching Costs

What users give up when they leave a competitor for you:
- **Data migration** effort
- **Learning curve** for new tool
- **Integration rebuilding**
- **Social/network** loss (e.g., leaving a platform with followers)

Higher competitor switching costs = harder to acquire users. Your job: reduce switching costs TO your product, increase switching costs FROM your product.

### Value Proposition

**Formula:** For [target user] who [need/problem], [product] is a [category] that [key benefit]. Unlike [alternative], we [key differentiator].

### Positioning Statement

**Template (Geoffrey Moore):**
> For [target customer] who [statement of need], [product name] is a [product category] that [key benefit]. Unlike [competitor], our product [primary differentiation].

**Example:**
> For busy professionals who struggle to stay informed, DailyBrief is a news aggregation app that delivers personalized 5-minute audio summaries. Unlike traditional news apps, DailyBrief uses AI to match your interests and fits into your commute.

### Message Map

```
Core Message (1 sentence)
    |
    +-- Proof Point 1 (data/feature that backs it up)
    +-- Proof Point 2
    +-- Proof Point 3

Top 3 Objections + Rebuttals
```

### Category Choice

You can either:
1. **Join an existing category** — easier to explain, but you're compared to incumbents
2. **Create a new category** — harder to explain, but you set the rules

---

## SESSION 5: Qualitative Research & Insight Synthesis

### Interview Design

**Good interview principles:**
- Ask about **past behavior**, not future intentions ("Tell me about the last time..." NOT "Would you use...?")
- Ask **open-ended** questions ("How do you currently...?" NOT "Do you like...?")
- Follow up with **"Why?"** at least 3 times to get to root cause
- **Don't pitch** your solution during the interview
- **Don't ask leading questions** ("Don't you think X would be great?")

### Recruiting

- Recruit from your **target segment**
- 5-8 interviews per segment is usually enough to find patterns
- Offer incentives proportional to their time
- Screen with qualifying questions to ensure fit

### Bias Avoidance in Research

- **Leading questions** — Don't suggest the answer in the question
- **Social desirability bias** — People say what they think you want to hear. Ask about behavior, not opinions.
- **Recall bias** — People misremember. Ask about recent, specific events.
- **Interviewer bias** — Your body language/reactions influence answers. Stay neutral.

### Affinity Mapping

1. Write each observation/quote on a sticky note
2. Group similar notes into **clusters**
3. Label each cluster with a **theme**
4. Look for patterns across clusters
5. Typically aim for 5-8 themes

### Journey Mapping

A visual representation of the user's experience across stages:

```
Stage:    Awareness -> Consideration -> Purchase -> Onboarding -> Usage -> Renewal/Churn
Actions:  [what they do at each stage]
Thoughts: [what they think]
Emotions: [happy/frustrated/confused]
Pain pts: [friction and problems]
Opptys:   [where you can improve]
```

### Turning Insights into Opportunities

**Insight format:** "[User segment] struggles with [problem] because [root cause], which leads to [negative outcome]."

**Opportunity format:** "How might we [solve the problem] for [user segment] so that [desired outcome]?"

---

## SESSION 6: Quantitative Research + Analytics Foundations

### Survey Basics

- Keep surveys **short** (< 10 minutes)
- Use a mix of **closed** (multiple choice, rating) and **open** (free text) questions
- **Don't lead** — neutral wording
- **Randomize** answer order to avoid order bias
- Pilot test before sending to the full audience
- Minimum **~100 responses** for quantitative confidence

### NPS / CSAT

| Metric | Question | Scale | Calculation |
|--------|----------|-------|-------------|
| **NPS** (Net Promoter Score) | "How likely are you to recommend us?" | 0-10 | % Promoters(9-10) - % Detractors(0-6) |
| **CSAT** (Customer Satisfaction) | "How satisfied are you with X?" | 1-5 | % who chose 4 or 5 |

- **NPS** = overall loyalty/brand health (lagging indicator)
- **CSAT** = satisfaction with specific interaction/feature (point-in-time)

### Quantitative Research Pitfalls

- **Self-selection bias** — Only motivated users respond to surveys
- **Small sample size** — Results not statistically meaningful
- **Correlation ≠ Causation** — Two metrics moving together doesn't mean one causes the other
- **Vanity metrics** — Total signups, page views without context are misleading
- **Survey fatigue** — Too many surveys = low quality responses

### Funnels

A **funnel** tracks users through a sequence of steps:

```
Visit Site (100%) -> Sign Up (30%) -> Activate (15%) -> Purchase (5%) -> Retain (2%)
```

- Each step has a **conversion rate**
- Find the biggest **drop-off** = biggest opportunity
- **Activation funnel** is critical: the steps from signup to first value moment

### Cohorts

Grouping users by a shared characteristic (usually sign-up date) and tracking their behavior over time.

**Example:** "Of users who signed up in January, what % are still active in Month 3?"

Cohorts help you see if your product is actually **improving** over time, not just growing.

### Instrumentation & Tracking Plans

An **event tracking plan** defines:
- **Event name** — what happened (e.g., `button_clicked`)
- **Properties** — context (e.g., `button_name: "checkout"`, `page: "cart"`)
- **Trigger** — when the event fires
- **Owner** — who maintains it

Without proper instrumentation, you're flying blind.

---

## SESSION 7: Metrics, North Star & Experimentation

### North Star Metric (NSM)

The **single metric** that best captures the core value your product delivers.

| Product | North Star Metric |
|---------|-------------------|
| Spotify | Time spent listening |
| Airbnb | Nights booked |
| Slack | Messages sent in teams |
| WhatsApp | Messages delivered |

**Good NSM criteria:**
1. Reflects **value delivered** to the user
2. **Leading indicator** of revenue
3. **Actionable** — teams can influence it
4. **Not a vanity metric** — measures real engagement

### Metrics Trees

```
North Star Metric
    |
    +-- Input Metric 1 (things you directly control)
    |       +-- Sub-metric 1a
    |       +-- Sub-metric 1b
    +-- Input Metric 2
    +-- Input Metric 3

Guardrail Metrics (things that must NOT get worse)
```

### Leading vs Lagging Indicators

| Leading (Predictive) | Lagging (Historical) |
|----------------------|---------------------|
| Activation rate | Revenue |
| Feature adoption | Churn rate |
| NPS score | LTV |
| Weekly active users | Annual revenue |

**Leading indicators** predict future performance. **Lagging indicators** confirm past results. PMs should focus on leading indicators because you can still influence them.

### Guardrail Metrics

Metrics that must NOT degrade while you optimize your primary metric.

**Example:** "We want to increase purchases (primary), but page load time (guardrail) must stay under 2 seconds, and support tickets (guardrail) must not increase."

Guardrails prevent you from gaming the primary metric at the expense of quality.

### A/B Testing Concepts

**What:** Randomly split users into groups. Group A sees the current version (control). Group B sees the change (variant). Compare results.

**Key concepts:**
- **Hypothesis** — "If we [change], then [metric] will [improve] because [reason]"
- **Effect size** — How big a difference you expect to detect
- **Sample size** — More users = detect smaller effects. Use a calculator!
- **Statistical significance (p-value)** — p < 0.05 means < 5% chance the result is random
- **Practical significance** — Is the effect big enough to matter even if statistically significant?
- **Duration** — Run long enough to capture weekly patterns (minimum 1-2 weeks)

**Common pitfalls:**
- Peeking at results too early
- Stopping when you see significance (multiple testing problem)
- Testing too many things at once
- Ignoring novelty effects

### OKRs (Objectives & Key Results)

- **Objective** = qualitative, inspiring goal ("Become the easiest onboarding experience")
- **Key Results** = quantitative, measurable outcomes (3-5 per objective)
  - "Reduce time-to-first-value from 10 min to 3 min"
  - "Increase activation rate from 30% to 50%"
  - "Decrease onboarding support tickets by 40%"

KRs should be **outcomes**, not tasks. "Launch feature X" is a task, not a KR.

---

## SESSION 8: Strategy + Business Case + Portfolio

### Vision Narrative

A compelling story of the future your product creates:
1. **Who** is the user?
2. **What problem** do they face today?
3. **What promise** does your product make?
4. **Why now?** — What changed that makes this possible/urgent?
5. **Strategy choice** — What's your unique approach?

### Strategy Choices

Strategy = a set of **coherent choices** about where to play and how to win.

Key strategic decisions:
- **Where to play** — Which segments, geographies, problems?
- **How to win** — Cost leadership, differentiation, or niche focus?
- **What to prioritize** — You can't do everything. What do you say NO to?

### Business Case Building

A business case justifies investment with numbers:

1. **Problem & opportunity size** (TAM/SAM/SOM)
2. **Proposed solution** (high-level)
3. **Revenue model** (how you make money)
4. **Cost structure** (build cost, operating cost)
5. **Financial projections** (revenue, costs, profit over 3 years)
6. **Key assumptions** (list them explicitly)
7. **Risks & mitigations**
8. **Ask** (what you need — budget, headcount, time)

### Unit Economics

| Metric | What It Means | Formula |
|--------|---------------|---------|
| **LTV** (Lifetime Value) | Total revenue from one customer over their lifetime | ARPU x Gross Margin x (1 / Churn Rate) |
| **CAC** (Customer Acquisition Cost) | Cost to acquire one customer | Total Sales+Marketing Spend / # New Customers |
| **LTV:CAC Ratio** | Is acquisition profitable? | LTV / CAC (target: > 3:1) |
| **Payback Period** | Months to recover CAC | CAC / Monthly Gross Profit per Customer |
| **Contribution Margin** | Revenue minus variable costs per unit | (Revenue - Variable Costs) / Revenue |

**Healthy benchmarks:**
- LTV:CAC > 3:1
- Payback period < 12 months
- Gross margin > 60% (for SaaS)

### Scenario Planning

Model **best case**, **base case**, and **worst case** scenarios:
- Vary key assumptions (conversion rate, churn, pricing)
- Identify which assumptions have the biggest impact
- Plan contingencies for worst case

### Portfolio Thinking & Kill Criteria

Not every product/feature should live forever.

**Kill criteria** = pre-defined conditions under which you STOP investment:
- "If activation < 10% after 90 days, kill it"
- "If CAC > $200 and not decreasing, kill it"
- "If NPS < 0 after 6 months, kill it"

Setting kill criteria **before** you start prevents sunk cost fallacy.

---

## SESSION 9: Business Models + Pricing & Packaging

### Business Model Types

| Model | How You Make Money | Example |
|-------|-------------------|---------|
| **Subscription (SaaS)** | Recurring fee | Netflix, Slack |
| **Freemium** | Free tier + paid upgrades | Spotify, Dropbox |
| **Marketplace** | Take rate / commission | Uber, Airbnb |
| **Advertising** | Sell user attention | Google, Instagram |
| **Transaction fee** | Per-transaction cut | Stripe, PayPal |
| **Hardware + Services** | Sell device + recurring service | Apple, Peloton |
| **Licensing** | One-time or periodic license fee | Microsoft Office (old model) |

### Value-Based Pricing

Price based on the **value delivered** to the customer, NOT your cost.

**Steps:**
1. Understand the customer's **willingness to pay** (WTP)
2. Quantify the **value** your product creates (time saved, revenue gained)
3. Price at a **fraction** of that value (customer should feel they're getting a deal)

**Example:** If your product saves a company $100K/year, pricing at $20K/year is a 5x ROI — easy to justify.

### Price Metric

The **unit** you charge for:
- Per seat (Slack)
- Per transaction (Stripe)
- Per usage/consumption (AWS)
- Per feature tier (most SaaS)
- Flat rate (Netflix)

**Good price metric:** Scales with the value the customer receives.

### Packaging Tiers

Typically 3 tiers (Good-Better-Best):

| Tier | Target | Strategy |
|------|--------|----------|
| **Free / Starter** | Individual users, leads | Land & expand, top of funnel |
| **Pro / Team** | Core target segment | Main revenue driver |
| **Enterprise** | Large orgs | High ACV, custom features |

### Monetization Guardrails

- Don't charge for things that block activation
- Don't surprise users with hidden costs
- Don't make free tier so good that nobody upgrades
- Don't make pricing so complex that users can't predict their bill

---

## SESSION 10: Discovery & Validation

### Assumption Mapping

Every product idea has assumptions in 3 categories:

| Category | Question | Example |
|----------|----------|---------|
| **Desirability** | Do users want this? | "Users will prefer audio over text news" |
| **Feasibility** | Can we build this? | "Our ML model can summarize articles accurately" |
| **Viability** | Can we make money from this? | "Users will pay $5/month for this" |

**Rank by risk:** High risk + high importance = test FIRST.

### Hypothesis Writing

**Format:** "We believe [assumption]. We will test this by [experiment]. We will know we are right when [measurable outcome]."

**Example:** "We believe users want audio news summaries. We will test this with a fake-door landing page. We will know we are right when > 10% of visitors click 'Get Early Access'."

### MVP Types

| Type | What It Is | When to Use | Speed |
|------|-----------|-------------|-------|
| **Prototype** | Clickable mockup (Figma) | Test usability & desirability | Fast |
| **Concierge MVP** | Manually deliver the service to a few users | Test if the value proposition works | Medium |
| **Fake-door / Smoke test** | Landing page / button for a feature that doesn't exist yet | Test demand before building | Fastest |
| **Wizard of Oz** | Looks automated to user, but human behind the scenes | Test experience without building tech | Medium |

**Key insight:** The best MVP is the **cheapest, fastest way to test your riskiest assumption**. It is NOT a "version 1 of the product."

### Ethical Testing

- Don't collect data without consent
- Debrief fake-door participants (tell them the feature doesn't exist yet)
- Don't manipulate vulnerable populations
- Respect opt-out requests

---

## SESSION 11: Prioritization + Stakeholders + Decision Memos

### Prioritization Frameworks

#### RICE

| Factor | Meaning | How to Score |
|--------|---------|-------------|
| **R**each | How many users affected in a time period? | Number (e.g., 1000 users/quarter) |
| **I**mpact | How much will it move the metric per user? | 3=massive, 2=high, 1=medium, 0.5=low, 0.25=minimal |
| **C**onfidence | How sure are you about R, I, E? | 100%=high, 80%=medium, 50%=low |
| **E**ffort | Person-months to build | Number (e.g., 2 person-months) |

**RICE Score = (Reach x Impact x Confidence) / Effort**

Higher score = higher priority.

#### Kano Model

Categorizes features by user satisfaction:

| Category | If Present | If Absent |
|----------|-----------|-----------|
| **Must-be (Basic)** | No extra satisfaction | High dissatisfaction |
| **Performance (Linear)** | More = more satisfied | Less = less satisfied |
| **Delighters (Excitement)** | High satisfaction | No dissatisfaction |

**Implication:** Fulfill all must-haves first, then compete on performance, then add delighters.

#### MoSCoW

- **Must have** — Non-negotiable for this release
- **Should have** — Important but not critical
- **Could have** — Nice to have if time permits
- **Won't have** — Explicitly out of scope (this time)

#### WSJF (Weighted Shortest Job First)

**WSJF = Cost of Delay / Job Duration**

Cost of Delay = User Value + Time Criticality + Risk Reduction

Higher WSJF = do first. Favors high-value, short-duration items.

### Stakeholder Mapping

Plot stakeholders on an **Influence x Interest** grid:

```
High Influence
    |
    | Manage Closely    |  Keep Satisfied
    | (high interest)   |  (low interest)
    |-------------------+------------------
    | Keep Informed     |  Monitor
    | (high interest)   |  (low interest)
    |
Low Influence
         High Interest        Low Interest
```

### Decision Memos

A 1-page document for making and communicating trade-off decisions:

1. **Context** — What's the situation?
2. **Options** — What are the 2-3 choices?
3. **Recommendation** — Which one and why?
4. **Trade-offs** — What you gain and lose with each option
5. **Risks** — What could go wrong?
6. **Reversibility** — Is this a one-way or two-way door?

**One-way door** = hard/impossible to reverse (launch, kill, pricing change) -> Decide carefully.
**Two-way door** = easily reversible (UI change, copy tweak) -> Decide fast.

---

## SESSION 12: Roadmapping + Product Ops

### Outcome Roadmaps (Now / Next / Later)

**NOT a feature list with dates.** An outcome roadmap communicates:

| Horizon | Timeframe | Confidence | Detail Level |
|---------|-----------|------------|-------------|
| **Now** | Current quarter | High | Specific outcomes + key bets |
| **Next** | Next quarter | Medium | Outcome themes |
| **Later** | 6+ months | Low | Strategic directions |

**Key principle:** The further out, the less specific. Don't promise features for Q3 when you're in Q1.

### Sequencing & Dependencies

- Sequence by **value + risk**: Do highest-value, highest-risk items first
- Map **dependencies** across teams
- Identify **critical path** — the longest chain of dependent tasks

### Roadmap Communication

Different audiences need different views:
- **Executives** — Strategic themes, business outcomes, bets
- **Engineering** — Technical dependencies, capacity, scope
- **Sales/CS** — Customer-facing features, timelines (careful with dates!)
- **Customers** — Problem themes, not feature promises

### Product Ops Cadence

| Ritual | Frequency | Purpose |
|--------|-----------|---------|
| **Standup** | Daily | Unblock delivery |
| **Sprint Review** | Bi-weekly | Demo + feedback |
| **Product Review** | Monthly | Metrics review + priority check |
| **Quarterly Planning** | Quarterly | Set OKRs, refresh roadmap |
| **Annual Strategy** | Yearly | Vision + strategy reset |

### RAID Log

| Letter | Meaning | Example |
|--------|---------|---------|
| **R**isks | Things that might go wrong | "API partner might change terms" |
| **A**ssumptions | Things we're assuming to be true | "Users have smartphones" |
| **I**ssues | Things that have already gone wrong | "Auth service is down" |
| **D**ependencies | Things we need from others | "Design team needs to deliver mocks by Friday" |

---

## SESSION 13: PRDs, UX Collaboration & Quality

### PRD Structure (Product Requirements Document)

1. **Problem statement** — What user pain are we solving?
2. **Goals** — What outcomes do we want?
3. **Non-goals** — What are we explicitly NOT doing?
4. **Scope** — What's in and out for this version?
5. **User stories** — As a [user], I want [action], so that [benefit]
6. **Acceptance criteria** — Specific, testable conditions for "done"
7. **Edge cases** — Unusual scenarios to handle
8. **Success metrics** — How we measure success
9. **Analytics requirements** — Events to track
10. **Non-functional requirements** — Performance, security, accessibility
11. **Open questions** — Unresolved decisions

### User Stories & Acceptance Criteria

**User story:** "As a returning customer, I want to reorder my last purchase so that I can save time."

**Acceptance criteria (AC):**
- Given I'm logged in, when I click "Reorder," then the same items are added to cart
- Given an item is out of stock, when I reorder, then I see a message and the available items are still added
- The reorder flow must complete in < 3 seconds

**Good AC is:** Specific, testable, covers happy path + edge cases.

### Non-Functional Requirements (NFRs)

| Category | Example Requirement |
|----------|-------------------|
| **Performance** | Page load < 2 seconds on 3G |
| **Security** | All data encrypted at rest and in transit |
| **Scalability** | Handle 10x current traffic |
| **Accessibility** | WCAG 2.1 AA compliant |
| **Reliability** | 99.9% uptime SLA |

### Usability Heuristics (Nielsen's Top 10)

1. **Visibility of system status** — User always knows what's happening
2. **Match between system and real world** — Use familiar language
3. **User control and freedom** — Easy undo/back
4. **Consistency and standards** — Same patterns everywhere
5. **Error prevention** — Design to prevent mistakes
6. **Recognition over recall** — Show options, don't make users remember
7. **Flexibility and efficiency** — Shortcuts for experts
8. **Aesthetic and minimalist design** — No unnecessary info
9. **Help users recognize and recover from errors** — Clear error messages
10. **Help and documentation** — Available when needed

### Story Mapping

```
User Journey (horizontal): Browse -> Select -> Checkout -> Receive

MVP Slice:      [basic browse] [add to cart] [simple checkout] [email confirmation]
V2 Slice:       [filters]      [wishlist]    [saved payment]   [tracking]
V3 Slice:       [recommendations] [reviews]  [gift wrap]       [returns]
```

Horizontal = journey steps. Vertical = depth/sophistication. MVP = thinnest horizontal slice that delivers end-to-end value.

---

## SESSION 14: Delivery, Releases & Experimentation Ops

### Agile Delivery Basics

- **Sprint** = fixed time period (usually 2 weeks)
- **Backlog** = prioritized list of work
- **Velocity** = how much work a team completes per sprint
- **Estimation** = story points or t-shirt sizes (S/M/L/XL)
- PM role: Prioritize backlog, clarify requirements, NOT manage engineers

### Feature Flags

A **feature flag** lets you turn features on/off without deploying new code.

**Uses:**
- **Gradual rollout** — Release to 5% of users, then 25%, then 100%
- **Kill switch** — Turn off a broken feature instantly
- **A/B testing** — Show feature to variant group only
- **Internal testing** — Enable for team before public release

### Release Planning (Phased Rollout)

```
Internal (dogfood) -> Alpha (employees) -> Beta (select users) -> GA (everyone)
```

| Phase | Audience | Goal |
|-------|----------|------|
| **Internal** | Team | Catch obvious bugs |
| **Alpha** | Company-wide | Broader testing |
| **Beta** | Select external users | Real-world validation |
| **GA** (General Availability) | All users | Full launch |

### Monitoring & Rollback

After launch, monitor:
- **Error rates** — Are crashes increasing?
- **Performance** — Is latency increasing?
- **Key metrics** — Is activation/retention dropping?
- **Support tickets** — Are complaints increasing?

**Rollback plan:** If [metric] degrades by [X%] for [Y minutes], roll back to previous version.

### Incident Learnings

When something breaks:
1. **Detect** — Monitoring alerts
2. **Respond** — Assign owner, communicate status
3. **Mitigate** — Fix or rollback
4. **Postmortem** — What happened, why, how to prevent (blameless!)
5. **Action items** — Concrete fixes with owners and deadlines

---

## SESSION 15: Enterprise, Platform & API Product Management

### B2B/Enterprise Specifics

| Aspect | What's Different |
|--------|-----------------|
| **Sales cycle** | Long (months), involves procurement, legal, IT |
| **Procurement** | RFPs, security questionnaires, compliance checks |
| **Pilots/POCs** | "Try before you buy" — 30-90 day trials |
| **Security reviews** | SOC 2, GDPR, HIPAA compliance required |
| **SLAs** | Guaranteed uptime, response times, support levels |
| **Renewals** | Revenue depends on retention, expansion, reducing churn |
| **Customization** | Enterprise customers demand custom features — resist wisely |

### Platform vs Product

| Product | Platform |
|---------|----------|
| Solves a specific user problem | Enables others to build products/solutions |
| Success = user satisfaction, engagement | Success = developer adoption, ecosystem growth |
| You control the experience | You provide building blocks, others control the experience |
| Example: Zoom (the app) | Example: Twilio (video API) |

### API Product Management

**DX (Developer Experience)** = the API equivalent of UX:
- Clear documentation
- SDKs in popular languages
- Sandbox/testing environment
- Quick time-to-first-API-call

**Versioning:**
- Use semantic versioning (v1, v2, v3)
- Support old versions for a defined period
- Communicate deprecation timeline clearly

**Deprecation:**
1. Announce deprecation (6-12 months notice)
2. Provide migration guide
3. Send reminders
4. Monitor usage of old version
5. Sunset (turn off)

### Ecosystem Metrics

| Metric | What It Measures |
|--------|-----------------|
| # of developers using API | Adoption |
| Time to first API call | Onboarding quality |
| API call volume | Usage/engagement |
| # of apps built on platform | Ecosystem health |
| Developer NPS | Satisfaction |

---

## SESSION 16: GTM, Growth & Post-Launch OS + Exec Communication

### Launch Tiering

| Tier | Scale | GTM Effort | Example |
|------|-------|-----------|---------|
| **Tier 1** | Major launch, new product/market | Full GTM: PR, events, enablement | New product line |
| **Tier 2** | Significant feature | Blog post, email, in-app | Major feature launch |
| **Tier 3** | Minor improvement | Changelog, in-app tooltip | UI improvement |

### GTM One-Pager

1. **Audience** — Who is this for?
2. **Promise** — What value does it deliver?
3. **Channels** — How will users learn about it?
4. **Messaging** — Key message + proof points
5. **Enablement** — What does Sales/CS/Support need to know?
6. **Launch tiering** — Alpha / Beta / GA phases
7. **Success metrics** — How do we know it worked?

### Enablement (Sales/CS/Support Readiness)

Before launch, equip internal teams:
- **Sales** — Pitch deck, objection handling, demo script, competitive positioning
- **CS (Customer Success)** — Feature walkthrough, FAQ, known limitations
- **Support** — Troubleshooting guide, known issues, escalation path

### Activation Funnel & Time-to-First-Value (TTFV)

**TTFV** = how long it takes a new user to experience the core value of your product.

**Shorter TTFV = higher activation = higher retention.**

Example funnel:
```
Sign Up -> Complete Profile -> First Action -> "Aha Moment" -> Habit Loop
```

Optimize each step. Remove friction. Add nudges (emails, tooltips, onboarding flows).

### Retention & Growth Loops

**Retention** = users coming back. Measured by:
- D1/D7/D30 retention (% of users active after 1/7/30 days)
- Cohort retention curves (should flatten, not drop to zero)

**Growth Loops** (self-reinforcing cycles):

```
User creates content -> Content attracts new users -> New users create content -> ...
```

Types:
- **Viral loop** — Users invite others (WhatsApp)
- **Content loop** — User-generated content drives SEO (Pinterest, Reddit)
- **Paid loop** — Revenue funds ads which drive users (subscription products)

### Churn Diagnostics

**Churn** = users leaving. To diagnose:
1. **When** do they churn? (Day 1? Month 3?)
2. **Who** churns? (Which segment?)
3. **Why** do they churn? (Survey, interviews, data)
4. **What** happened before churn? (Last actions, support tickets)

### Executive Communication

**Weekly Business Review Slide:**

| Section | Content |
|---------|---------|
| **Metric** | NSM + key metrics (vs target, vs last week) |
| **Insight** | What's driving the metric (up or down)? |
| **Action** | What we're doing about it |
| **Ask** | What we need from leadership (decision, resources, unblock) |

Keep it to ONE slide. Executives want signal, not noise.

---

## QUICK REFERENCE: KEY FRAMEWORKS CHEAT SHEET

| Framework | Use For | Remember |
|-----------|---------|----------|
| **JTBD** | Understanding user needs | "When I [situation], I want to [motivation], so I can [outcome]" |
| **TAM/SAM/SOM** | Market sizing | TAM > SAM > SOM; bottom-up is more credible |
| **RICE** | Prioritization | (Reach x Impact x Confidence) / Effort |
| **Kano** | Feature categorization | Must-be, Performance, Delighters |
| **MoSCoW** | Scope decisions | Must, Should, Could, Won't |
| **OKR** | Goal setting | Objective (qualitative) + Key Results (quantitative outcomes) |
| **Now/Next/Later** | Roadmapping | Decreasing confidence over time |
| **RAID** | Risk tracking | Risks, Assumptions, Issues, Dependencies |
| **LTV/CAC** | Unit economics | LTV:CAC > 3:1 is healthy |
| **NPS** | Customer loyalty | Promoters - Detractors |
| **Nielsen's 10** | Usability review | Visibility, consistency, error prevention... |

---

## GLOSSARY OF KEY TERMS

- **Activation** — The moment a user first experiences core product value
- **ARPU** — Average Revenue Per User
- **Churn** — Rate at which customers leave
- **Cohort** — Group of users sharing a characteristic, tracked over time
- **Feature Flag** — Toggle to enable/disable features without deployment
- **Guardrail Metric** — Metric that must not degrade when optimizing another
- **ICP** — Ideal Customer Profile (B2B target company description)
- **JTBD** — Jobs To Be Done framework
- **Leading Indicator** — Metric that predicts future outcomes
- **Lagging Indicator** — Metric that confirms past results
- **MVP** — Minimum Viable Product (cheapest test of riskiest assumption)
- **North Star Metric** — Single metric reflecting core value delivery
- **NFR** — Non-Functional Requirement (performance, security, etc.)
- **PRD** — Product Requirements Document
- **RACI** — Responsible, Accountable, Consulted, Informed (role clarity matrix)
- **SLA** — Service Level Agreement
- **TTFV** — Time To First Value
- **WTP** — Willingness To Pay

---

*Good luck on your exam!*
