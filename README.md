# AI Humanizer Skill

![MIT license](https://img.shields.io/badge/license-MIT-blue.svg)

AI Humanizer Skill rewrites text to a specific house style: plain language, active voice, no AI-tell vocabulary, no em dashes, no generic headings, no visible drafting process. Because it is just Markdown, it works with any agent that supports skills.

This is a separate skill from [humanizer](https://github.com/blader/humanizer), and intentionally so. Humanizer removes AI writing patterns while staying neutral about tone: it adapts to whatever voice the source calls for. AI Humanizer Skill applies one specific, opinionated house style on top of that idea: a fixed banned-word list, a hard no-em-dash rule, no generic section headings, and a strict "final text only" output contract for pasted text. Use humanizer when you want general AI-pattern cleanup that preserves the original voice. Use this skill when the output needs to match one house style consistently, especially blog posts, GTM copy, marketing content, and social posts.

## How it works

The rule set is original house style, built around 13 named patterns adapted in part from Wikipedia's ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup, and reframed for GTM and marketing copy rather than encyclopedia prose.

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

It does not make things up. A name, number, date, quote, or other factual detail must come from the source text or the writer, never invented to make a rewrite read more smoothly. If a sentence needs a missing detail, the skill asks for it or rewrites around it instead of guessing.

Unlike humanizer, this skill does not show its work by default. It writes a draft, checks that draft silently against every rule below, and hands back only the finished text: no visible draft, no list of what still sounded artificial, no offer to continue. Ask explicitly if you want to see the reasoning or the draft stage. Point it at a file and it rewrites only the prose in place, leaving code, data, frontmatter, and link targets untouched.

## Usage

Call the skill directly:

```
/ai-humanizer-skill

[paste your text here]
```

Or ask in plain language:

```
Rewrite this in my house style: [your text]
```

To rewrite a file, give it the path:

```
Rewrite the copy in docs/launch-post.md in my house style
```

### Match your voice

If you want the rewrite to sound more like a specific writer, include a sample of their past work:

```
/ai-humanizer-skill

Here's a writing sample for voice matching:
[paste 2-3 paragraphs]

Now rewrite this:
[paste text to rewrite]
```

The skill reads the sample for sentence length, word choice, and paragraph rhythm, then applies that voice within the house style rather than defaulting to a generic "direct and opinionated" tone.

## Rule categories

| Category | Covers |
|---|---|
| Sentence rules | Active voice, no em dashes, no semicolons, varied sentence length, no "not just X, but Y" |
| Word rules | A fixed cut list (can, may, just, delve, unlock, tapestry, and similar), when to repeat a word instead of forcing a synonym, hyphenation in predicate position |
| Structure rules | No generic headings ("Key Takeaways," "Why It Matters"), no forced rule of three, no symmetrical bullet lists, varied paragraph openers |
| Tone rules | Take a position when the facts support one, cut hedging and filler transitions, don't over-explain the obvious |

## The 13 patterns

### Content and proof patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Significance inflation** | "marked a pivotal moment in the company's evolution, underscoring its commitment to growth" | "The company moved to a bigger office because the old one couldn't fit the new hires." |
| 2 | **Promotional language** | "Our cutting-edge platform delivers a seamless, powerful experience" | "The platform syncs data across tools in under a second." |
| 3 | **Notability name-dropping** | "Featured in TechCrunch, Forbes, and Business Insider, with an active presence everywhere" | "TechCrunch called our launch 'the fastest-growing dev tool of 2026.'" |
| 4 | **Copula avoidance** | "Our onboarding flow serves as the foundation... and boasts a 40% faster completion rate" | "Onboarding is the foundation of the customer journey. It's 40% faster than before." |
| 9 | **Generic conclusion** | "The future looks bright as the company continues its journey toward excellence" | "The company plans to open two more offices next year." |

### Structure patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 5 | **Forced rule of three** | "The update improves speed, reliability, and user satisfaction" | "The update cut load time by 40%." |
| 6 | **Negative parallelism** | "This isn't just a pricing change, it's a complete rethink of the product" | "The pricing change reflects a rethink of the whole product." |
| 8 | **False range** | "The platform handles everything from onboarding to enterprise security" | "The platform handles onboarding, permissions, and audit logs." |
| 13 | **Fragmented header** | "## Performance\n\nSpeed matters.\n\nUsers who hit a slow page tend to leave." | "## Performance\n\nUsers who hit a slow page tend to leave." |

### Voice and rhythm patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 7 | **Elegant variation** | "The founder built the tool. The entrepreneur later sold the company. The businesswoman now advises startups." | "The founder built the tool, sold the company, and now advises startups." |
| 10 | **Manufactured punchlines** | "Then the update shipped. No warning. No changelog. No rollback plan." | "The update shipped with no warning, no changelog, and no rollback plan, which is why the on-call engineer found out from a customer complaint." |
| 11 | **Aphorism formula** | "Speed is the currency of modern software." | "Users leave a page that takes more than three seconds to load." |
| 12 | **Fake-candid opener** | "Is it worth the price? Honestly? It depends." | "Whether it's worth the price depends on how often you'll use it." |

Full rule text, the "what not to flag" guardrails, and the complete banned-word list live in [SKILL.md](SKILL.md).

## Full example

**Before (AI-sounding):**
> We're thrilled to announce a pivotal moment in our platform's evolution: the launch of Workflow Automations. This groundbreaking feature isn't just an update, it's a complete reimagining of how teams get work done.
>
> Our cutting-edge automation engine boasts a seamless, powerful integration with your existing tools, delivering a diverse array of triggers, actions, and conditions. Whether you're managing onboarding, approvals, or reporting, Workflow Automations empowers your team to eliminate friction, unlock efficiency, and drive results.
>
> As featured in TechCrunch, Product Hunt, and The Verge, we've built something the industry is already talking about. Early adopters report significant, transformative gains in speed, accuracy, and satisfaction.
>
> Is it a game-changer? Honestly? We think so.
>
> The future of work is automated, and we couldn't be more excited to be at the forefront of this evolving landscape. Get started today and experience the difference for yourself!

**After:**
> Workflow Automations is live today. You can now trigger actions in Zendesk, Slack, and Notion from a single rule, without writing a script.
>
> Before this, most teams stitched automations together with Zapier and a few brittle webhooks. That works until the third integration breaks and nobody remembers why. Workflow Automations replaces that stack with one rules engine: pick a trigger, pick a condition, pick an action.
>
> TechCrunch called it "a serious Zapier alternative built for support teams," which is close to what we were going for.
>
> One early customer cut their onboarding checklist from six manual steps to one automated one. Another uses it to route escalations by account tier, which used to be a Slack bot someone built in a hackathon and never touched again.
>
> It's live on every paid plan today. Setup takes about ten minutes.

## Sources

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) informed patterns 1, 2, 3, 4, 6, 7, 8, and 9.
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) maintains that page.
- The word rules, structure rules, tone rules, and patterns 10, 11, 12, and 13 are original house style, not sourced from Wikipedia.

## Version history

<details>
<summary>Show release notes</summary>

- **1.2.0** - Renamed the skill and repository to AI Humanizer Skill (was Copy & Humanizer). Added file mode and embedded mode to the output contract, and an explicit rule against inventing facts, names, numbers, or dates. No change to the 13 patterns.
- **1.1.0** - Added patterns #3 (notability name-dropping) and #4 (copula avoidance), found by cross-checking the rule set against Wikipedia's "Signs of AI writing." Numbered all patterns with `### N.` headers. Added the Claude plugin manifests, a package validator, and a CI workflow that runs on every push and pull request. 13 patterns total.
- **1.0.0** - Initial release. House style rules, 11 named failure patterns with before/after examples, silent draft-and-check process, voice calibration.

</details>

## License

MIT

## Installation

Install AI Humanizer Skill with the Skills CLI:

```bash
npx skills add johnakande/ai-humanizer-skill --global
```

Leave off `--global` to install it only in the current project. Add `--agent <name>` or `--agent '*'` to choose which agents receive it, then reload their skills.

Claude Code 2.1.142 or newer can install the plugin instead:

```
/plugin marketplace add johnakande/ai-humanizer-skill
/plugin install ai-humanizer-skill@ai-humanizer-skill
```

The plugin command is `/ai-humanizer-skill:ai-humanizer-skill`.

In Claude Desktop, download this repository as a ZIP and upload it as a skill.

### Manual install (Claude Code)

Clone directly into Claude Code's skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/johnakande/ai-humanizer-skill.git ~/.claude/skills/ai-humanizer-skill
```

Or copy the skill file manually if you already have this repo cloned:

```bash
mkdir -p ~/.claude/skills/ai-humanizer-skill
cp SKILL.md ~/.claude/skills/ai-humanizer-skill/
```

### Manual install (OpenCode)

```bash
mkdir -p ~/.config/opencode/skills
git clone https://github.com/johnakande/ai-humanizer-skill.git ~/.config/opencode/skills/ai-humanizer-skill
```

> **Note:** OpenCode also scans `~/.claude/skills/`, so if you use both tools, a single clone into `~/.claude/skills/ai-humanizer-skill/` is enough.

For a manual install into any other agent, copy `SKILL.md` into that agent's skill folder.
