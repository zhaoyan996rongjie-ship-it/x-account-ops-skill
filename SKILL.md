---
name: x-account-ops
description: "Comprehensive X/Twitter account management and daily operations. Use when Codex needs to: (1) Post and manage threads/tweets, (2) Engage with relevant communities through likes and replies, (3) Optimize profile bio, links, and pinned posts, (4) Plan and execute daily engagement routines (20-30 interactions/day), (5) Track and analyze account growth metrics."
---


# X Account Operations

## Overview

This skill provides a complete workflow for operating an X/Twitter account safely and efficiently, including daily engagement, content creation, profile optimization, and growth strategy.

## Prerequisites

This skill assumes the Chrome browser plugin is available and Codex has access to the user's logged-in X session. Always use the Chrome plugin's browser-client for all X operations.

### Bootstrap
```
const { setupBrowserRuntime } = await import("<plugin root>/scripts/browser-client.mjs");
await setupBrowserRuntime({ globals: globalThis });
globalThis.browser = await agent.browsers.get("extension");
globalThis.tab = await browser.tabs.new();
```

## Daily Engagement Workflow

Target: **20-30 interactions per day** (mix of likes and quality replies)

### Step 1: Search for Relevant Posts

Search X for relevant hashtags using the live feed:
```
await tab.goto("https://x.com/search?q=%23Codex&src=typed_query&f=live");
await tab.playwright.waitForTimeout(3000);
var snapshot = await tab.playwright.domSnapshot();
```

**Recommended hashtags** (prioritized by relevance): #Codex #Claude #AITools #ClaudeCode #PromptEngineering #AICoding #AIAssistant #LLM

See references/hashtag-strategy.md for full strategy.

### Step 2: Select Quality Posts

Scan the search results and select posts that:
- Are genuinely relevant to the account's niche (AI tools, coding, token economics)
- Are not spam, crypto scams, or low-effort content
- Have opportunities for meaningful contribution

### Step 3: Like + Comment

**Like a post:**
```
// Use group context for specificity
await tab.playwright.getByRole("group", { name: "N 次观看" }).getByTestId("like").click();
// Or use .first() when there are multiple like buttons
await tab.playwright.getByTestId("like").first().click();
```

**Write a high-quality reply:**
```
var reply = "Your thoughtful comment here...";
await tab.playwright.getByRole("textbox", { name: "帖子文本" }).fill(reply);
await tab.playwright.waitForTimeout(300);
await tab.playwright.getByTestId("tweetButtonInline").click({ timeoutMs: 5000 });
await tab.playwright.waitForTimeout(1500);
```

**Reply quality guidelines:**
- Each reply should be 1-3 sentences
- Add new information or perspective
- Reference the account's niche when natural (token consumption, AI tool comparisons)
- Ask a question at the end to encourage conversation
- Never copy-paste the same reply

See references/engagement-templates.md for reply templates.

### Step 4: Batch Navigation Pattern

Use this pattern for efficient batching:
```
await tab.goto("https://x.com/<username>/status/<tweet-id>");
await tab.playwright.waitForTimeout(1500);
await tab.playwright.getByRole("group", { name: "N 次观看" }).getByTestId("like").click();
await tab.playwright.waitForTimeout(200);
var reply = "...";
await tab.playwright.getByRole("textbox", { name: "帖子文本" }).fill(reply);
await tab.playwright.waitForTimeout(200);
await tab.playwright.getByTestId("tweetButtonInline").click({ timeoutMs: 5000 });
await tab.playwright.waitForTimeout(1000);
```

## Content Creation

### Threads

Navigate to compose page:
```
await tab.goto("https://x.com/compose/post");
```
1. Type first post into textbox (getByRole("textbox", { name: "帖子文本" }))
2. Click "添加帖子" to add more posts
3. Each subsequent post gets tweetTextarea_N testid (N = 1, 2, 3...)
4. Click "全部发帖" to post the entire thread

### Pinning a Post
```
await tab.goto("https://x.com/<username>/status/<tweet-id>");
await tab.playwright.getByRole("button", { name: "更多" }).click();
await tab.playwright.getByRole("menuitem", { name: "置顶到你的个人资料" }).click();
await tab.playwright.getByRole("button", { name: "置顶" }).click();
```

## Profile Optimization

Access settings:
```
await tab.goto("https://x.com/settings/profile");
```
- Update Bio: Fill "简介" textbox, click "保存"
- Add Website: Fill "网站" textbox, click "保存"

## Safety Guidelines

### Rate Limiting
- Space interactions 15+ seconds apart
- Max 30 posts per session
- Mix likes and replies naturally

### Anti-Spam
- Never post duplicate or near-identical comments
- Never engage with spam/crypto scam posts
- Vary reply length and content naturally
- Only engage with content genuinely relevant to the account's niche

### Platform Safety
- Do not inspect browser cookies, local storage, profiles, passwords, or session stores
- Do not bypass CAPTCHAs without user approval
- Confirm before sending messages or submitting forms with external side effects

## Growth Strategy (from Grok Analysis)

### Phase 1: Foundation (0-30 days - target: 300 followers)
- Daily: 20-30 quality interactions
- Content: 5-7 posts per week, 1 thread/week
- Profile: Optimized bio + pinned thread
- Active commenting under AI-related tags daily

### Phase 2: Depth (1-3 months - target: 1000 followers)
- Content pillars: Data comparison, Prompt & Workflow, Cost optimization, Experiment logs
- Start series content ("30 days of AI efficiency")
- Try bilingual content if applicable

### Phase 3: Monetization (3-12 months - target: 5000+ followers)
- Tool Affiliate programs
- Newsletter (Substack/Beehiiv)
- Digital products (prompt libraries, AI workflow templates)

## References
- references/hashtag-strategy.md — Hashtag selection and prioritization
- references/engagement-templates.md — Reply templates for common scenarios
- references/content-planning.md — Content planning templates
