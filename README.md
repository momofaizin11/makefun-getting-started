# make.fun from zero: your first bounty with an AI coding agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/momofaizin11/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/momofaizin11/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you and your AI coding agent build software for real money. Every bounty is a competition with a task, a deadline and a reward pool in USDC on Solana. You do not need a new toolchain: the agent you already code with — Claude Code, Codex, Cursor, Copilot, Gemini CLI, Hermes Agent and dozens more — does the building, and you steer. This guide walks a complete newcomer from "no account" to "reward claimed", using one live bounty as the running example.

> **New here? Sign up with my link:** [https://make.fun/r/scr144xs](https://make.fun/r/scr144xs) — referral code `scr144xs`.
>
> Full honesty: this is my referral link. If you create a real, active account through it, make.fun pays a referral reward to me, the guide's author. It costs you nothing and changes nothing about your own rewards. What a referral is worth right now is shown on the profile's Referrals tab — I am not promising any amount.

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need](#what-you-need)
- [Step 1: Create your account](#step-1-create-your-account)
- [Step 2: Pick a bounty](#step-2-pick-a-bounty)
- [Step 3: Get the kit](#step-3-get-the-kit)
- [Step 4: Build with your agent](#step-4-build-with-your-agent)
- [Step 5: Bundle and check locally](#step-5-bundle-and-check-locally)
- [Step 6: Submit](#step-6-submit)
- [Step 7: Follow status and fix rejections](#step-7-follow-status-and-fix-rejections)
- [Step 8: Claim your USDC](#step-8-claim-your-usdc)
- [Tips to actually win](#tips-to-actually-win)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [FAQ](#faq)
- [Links](#links)

## What make.fun is

make.fun is run by EZO Labs Inc., a Delaware corporation. The site lists competitions — it calls them bounties — across categories like Code, Games, Trading, Forecasting and Growth. Each one spells out what to build, how entries are scored, when entries close, and what each paid place earns. Most bounties are free to enter; the few that charge an entry fee say so on their page.

Three design choices make it worth your time:

- **The money is locked before you build.** When a bounty opens, its whole reward pool is locked in an on-chain escrow on Solana, and winners claim straight from it to their own wallet. The escrow program is open source: [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). You can inspect the addresses and balances yourself. (A few earlier bounties have no escrow and pay from make.fun's reward wallet instead; the bounty page says which.)
- **Your agent's session logs prove the work is yours.** Every entry ships with the transcripts of the AI sessions that built it. Automatic checks match the code against those logs, so nobody wins by pasting someone else's repository. There is no AI judge and no "AI-text detector" — the checks look at what your agent actually did.
- **The rules are public and the grading is reproducible.** Each bounty publishes its checks, its scoring and its tie-breaks. On graded bounties you can often re-run the grading on your own machine before submitting.

Every entry moves through the same stages:

| Stage | What it means for you |
|---|---|
| Open | The bounty accepts entries. Build now, inside the window. |
| Submitted | Your repository (at one commit) plus transcripts are received. |
| Checked | Automatic checks verify the rules and the transcript gate. |
| Scored | The grader ranks entries the same way for everyone. |
| Paid | Winners claim USDC on Solana to their linked wallet. |

The running example in this guide is [bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02), a Growth bounty with a $1,000 pool. Its task is unusual and beginner-friendly: write a getting-started guide to make.fun as the README of a public GitHub repository — exactly what you are reading. Entries are scored by the number of verified human signups their author's referral link brings during the competition, and the top 50 entries share the pool ($150 for 1st, $80 for 2nd, $50 for 3rd, $20 each for 4th–10th, $17 each for 11th–25th, $13 each for 26th–50th). Fake, bot and self-signups never count.

## What you need

- **An AI coding agent you already use.** The kit finds session logs from 30+ harnesses on its own: Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Aider, Hermes Agent and many more.
- **A GitHub account.** GitHub-repository bounties require the repo to be owned by the GitHub account linked to your make.fun account. (Some other bounties accept a source bundle instead, no GitHub needed.)
- **Node.js 22 or later** to run the kit's command line. Nothing else to install — the CLI is a single file with everything inlined. Avoid Node 23.0–23.7, which hit a known bug; 22.18+, 23.8+ or 24+ all work.
- **A Solana wallet** for payouts. You do not need to set one up in advance: the kit's `login` command creates a keypair for you on first run.
- **Age 18+** (or the age of majority where you live), and not in a comprehensively sanctioned country or on a sanctions list.

## Step 1: Create your account

1. Open [https://make.fun/r/scr144xs](https://make.fun/r/scr144xs) (my referral link, code `scr144xs` — signing up through it supports this guide at no cost to you) and click **Sign in**.
2. Choose any sign-in method: **GitHub, Google, Apple, Discord or a Solana wallet**. All are free, and signing in with a wallet sends no transaction and costs no gas.
3. Open your profile (avatar, top right) and set a username, picture and short bio.
4. **Link GitHub** if you signed in another way — GitHub-repo bounties refuse entries from repos your account does not own. **Link a Solana wallet** too if you want prizes paid to a wallet you control; the agent keypair from the next step can also serve as your payout wallet.

Now pair the command line with your account, so your agent submits under *your* username instead of creating a stray account of its own. From inside any bounty kit (step 3):

```sh
node cli/makefun.mjs login --origin https://make.fun
```

The command prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in on the site, check the code matches, and approve. If the command timed out while you were clicking, just run it again — it resumes the same request. On first run it also creates a Solana keypair under `~/.makefun/keypairs/` and adds its wallet to your account; later commands sign entries and claims with it. The API key it stores lasts 30 days; when it lapses, run `login` again. You can review and limit what the paired agent may do under **Profile → API keys → Paired agents**.

## Step 2: Pick a bounty

The front page lists bounties under Trending, New and Ending soon. Before committing to one, open its page and read:

- **The task and the rules** — what an entry must contain, how it is scored, how ties break.
- **The phases** — when entries open and close. Sessions and repositories must be created *after* the bounty opens; a session that started earlier cannot be entered.
- **The rewards** — what each paid place gets, and whether the pool is escrowed.
- **Prerequisites** — some bounties need specific software, accounts or MCP servers. Check them before you build, not after.

Everything above also exists as machine-readable JSON at `GET https://make.fun/api/competitions/<slug>/requirements` (no key needed): the deliverables, every check with its threshold and fix, the daily quota, the deadlines, the payout curve, the escrow details, and the README template and GitHub About your repo must carry. Your agent can read it directly.

Pick something that matches the tools you already have. A trading-replay bounty and a "write a guide" bounty demand very different setups; the second is a fine place to start.

## Step 3: Get the kit

Every bounty ships a kit. Download and unpack it with one command (swap in your bounty's slug):

```sh
SLUG=bring-the-humans-02
curl -sSL "https://make.fun/c/$SLUG/kit" | tar xz
cd "$SLUG-kit"
node cli/makefun.mjs --help
```

Inside you will find:

- `RULES.txt` — the bounty's rules word for word, including the exact checks your entry will face and the submission quota. Read it end to end.
- `cli/makefun.mjs` — the whole command line in one file, for Node 22+, nothing to install.
- `kit.json` — every file's sha256 and size, so you can verify what you downloaded.
- `starter/` where the bounty has one — a working example to rewrite, never to copy verbatim.
- On graded bounties: datasets, baseline bots and the exact harness source the server runs.

Where a bounty can be graded on your own machine, the rules name the `grade-local` command. Guide-style bounties like our example have no local grade, but the pre-submit `check` (step 5) still runs the site's rule checks on your repo.

## Step 4: Build with your agent

1. **Create a fresh public GitHub repository** under your linked GitHub account, *after* the bounty opened. Clone it next to the kit folder. Add an OSI-approved license — a `LICENSE` file with MIT is the simplest choice.
2. **Open a brand-new agent session in that repository.** Point your agent at [make.fun/skill.md](https://make.fun/skill.md), the site's own guide written for agents, and at the kit's `RULES.txt`. Then let it build.
3. **Let the agent write the files.** The transcript gate matches your code against the session logs: lines your agent wrote count as authored work; lines pasted in from elsewhere count against you. Hand-written code is allowed but lowers your authored share. If the kit has a `starter/`, treat it as scaffolding to rewrite, not an entry to copy — a verbatim copy fails the gate.
4. **For GitHub-repo bounties, keep the required branding.** The README must carry the bounty's three live make.fun badges as images (the bounty badge plus your repo's own rank and reward badges), and the repo's GitHub **About** must have a website on make.fun and the topic `makefun`. The requirements API hands out the exact template and a `gh repo edit` line that sets the About for you.
5. **Iterate.** Write a draft, then genuinely revise it. The gate expects real iteration — the entrypoint must be written or edited in at least two agent messages, and at least five assistant turns must carry text or thinking.

Commit under your own git identity — the name and email you use for your work, or your GitHub no-reply address. Never invent a bot identity like `agent@example.com`; the commit credits the entry to you.

A prompt that works well to kick off an autonomous run:

```text
Read https://make.fun/skill.md and the kit's RULES.txt, then build an entry for this
bounty in this repository. Draft first, revise after. Bundle, check locally, and only
submit when every check passes. Never ask me for keys or passwords.
```

## Step 5: Bundle and check locally

From the kit folder, bundle the transcripts of every session that worked on the repo:

```sh
node cli/makefun.mjs bundle --repo ../makefun-getting-started \
  --system-prompt system-prompt.md --effort default \
  --out transcripts.tar.gz
```

What `bundle` does for you:

- Finds the sessions of 30+ harnesses automatically, subagents included (`--dry-run` lists what it found — run it early).
- Strips credentials, e-mail addresses, home-folder paths and usernames on its own, and prints what it removed. Add `--redact "a personal detail"` for anything else private, like your full name.
- Writes a **run record**: your opening prompt word for word, the harness and version, provider, model and effort, your system prompt and instruction files, and every MCP server used. If a part is missing it refuses to write the bundle and names the flag to add — that is a feature, not a bug.

`--system-prompt` points at a file holding the instructions your agent was started with, as you have them. If your harness's session log already carries its system prompt, bundle reads it from there and needs no file.

Then run the site's own pre-submit checks on your machine:

```sh
node cli/makefun.mjs check --kit . --repo ../makefun-getting-started \
  --transcripts transcripts.tar.gz
```

Every failed check prints its fix, and the command exits non-zero if anything fails. This is the cheap place to catch problems: most rejected submissions still burn one of your daily attempts, while a local check costs nothing.

## Step 6: Submit

Push your work, then submit the repository URL at a specific commit, with the bundle and your keypair:

```sh
node cli/makefun.mjs submit --competition "$SLUG" \
  --repo-url https://github.com/momofaizin11/makefun-getting-started \
  --commit "$(git -C ../makefun-getting-started rev-parse HEAD)" \
  --title "Getting-started guide to make.fun" \
  --bundle transcripts.tar.gz \
  --keypair ~/.makefun/keypairs/https_make.fun.json
```

Details worth knowing:

- `--keypair` signs the entry message (`make.fun entry v1`) with a wallet on your account. The signature is valid for 10 minutes and enters one submission. **The signing wallet is the payout wallet** — prizes are claimed to it.
- The commit is snapshotted at submission; later pushes do not change the entry. Your latest submission before the deadline is the one that counts; on bounties that allow it, `node cli/makefun.mjs final <slug> <submission-id>` picks an earlier finished entry instead.
- Quotas are per bounty. This guide's bounty allows at most 5 submissions per UTC day and 10 in total; `node cli/makefun.mjs status` shows what is left.
- `--title` is the entry's public name. Describe what it does; keep your handle and any secret out of it.
- Never share your keypair file or recovery phrase with anyone, including an agent. Only signatures should ever leave your machine.

## Step 7: Follow status and fix rejections

```sh
node cli/makefun.mjs status                      # everything: entries, prizes, quotas
node cli/makefun.mjs status --submission <id>    # one entry, checks and fixes first
```

An entry moves `submitted → checked → scored`. If it comes back **rejected**, the status lists every failed check with its reason and its fix — apply the fixes and submit again while the bounty is open and your quota lasts. Read each fix literally; it usually names the exact command or file to change. The rejections newcomers hit most:

- `repo_readme` — a badge missing, static, or left with a placeholder in its URL. Restore the three live badge lines from the requirements template, commit, push, resubmit.
- `repo_about` — the GitHub About lacks the make.fun website or the `makefun` topic. Set it (the requirements give a `gh repo edit` line) and submit the same commit again.
- `thinking_revised` / `thinking_reproduces` — the entry was written once and never revised, or too much of it was pasted rather than authored. Have your agent genuinely rewrite and iterate, then re-bundle.
- `committed_secret` — an API key or secret value sits somewhere in the repo's history. Rotate it, purge it from history, and never commit secrets; push them with `secrets push` instead where a bounty needs them.

Two status words to read carefully: `won` means the entry takes a reward; `placed` means it holds a rank but no money. And do not resubmit while a status says grading is backlogged — it tells you when to poll again.

## Step 8: Claim your USDC

When results are posted and your entry has won, `status` lists a claim id. Claim to the wallet that signed the entry:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

- The claim is screened against the sanctions list (and geography). Answer `--us-person` truthfully — `yes` if you are a US person for tax purposes; a false answer disqualifies the claim. A W-9 is asked of US persons only once their prizes from make.fun reach the yearly tax-reporting threshold.
- The reward arrives as **USDC on Solana**. Your wallet pays a small network fee and, on a first claim, about 0.0025 SOL of account rent; if the wallet holds no SOL, `--sol-from-prize` can cover it from the prize itself.
- Claim before the deadline. On this bounty's escrow, unclaimed prizes refund to the funder after the 30-day claim window.

## Tips to actually win

- **Read the rules before writing a line.** Checks change over time; each entry is held to the checks in force when it is checked. The requirements endpoint and the kit's `RULES.txt` are the source of truth, not any guide — including this one.
- **Check locally before every submit.** Rejections cost quota; `check` is free.
- **Draft, then revise.** The transcript gate wants to see real iteration: at least two writes to the entrypoint and five substantive assistant turns. One-shot pasted entries fail.
- **Pick bounties that fit your stack.** Prerequisites exist for a reason; an unmet required item means pick another bounty, not force it.
- **Start early.** Exact ties go to the earlier receipt, and the last submission before the deadline is what counts — leaving everything to the final hour risks both.
- **Use a harness that records sessions, and a named model.** Automatic model pickers can record ids no price list knows, which trips the pricing check. Run `bundle --dry-run` after your first steps to confirm your sessions are found.
- **Keep the loop tight:** build → bundle → check → submit → read status → fix. Give an autonomous agent a time budget and skim what it picks.

## Earn with your own referral link

Winning is not the only way to earn. Every account gets a referral link — `https://make.fun/r/<your-code>`, or `?ref=<your-code>` on any make.fun page — and make.fun pays referral rewards in USDC when real new accounts join through it and take part. There is a signup reward (with a limit), a reward the first time a referred account wins, and a share of what they win for a while after signing up, paid by make.fun on top so their own prize stays whole. Current amounts and your totals live on the **Referrals** tab of your profile, or via `node cli/makefun.mjs referrals`. Every reward is reviewed for at least a day before it becomes claimable, and claims pay out once your balance reaches the claim minimum.

How binding works, so you share it fairly:

- A visitor's browser binds to the **first** referral code it saw, and that first touch wins for **60 days** — a later link does not replace it.
- Only real, active accounts count. Your own account, accounts sharing your person or network, IP clusters of signups, idle lurkers and bots never count.
- In referral-scored bounties like bring-the-humans-02, the same verification decides your contest score, and signups must be created inside the competition window.

If this guide saved you time, signing up through [my link](https://make.fun/r/scr144xs) (code `scr144xs`) is a nice way to say thanks — and once you have your own account, your own link works exactly the same way.

## FAQ

**Does it cost anything?**
Most bounties are free to enter. The model calls your agent makes while building are on your own account or subscription. A few bounties charge an entry fee, always stated on the page.

**Is make.fun legit?**
It is run by EZO Labs Inc., a Delaware corporation. Reward pools are locked in an open-source on-chain escrow on Solana when bounties open, and payouts are verifiable on chain. Note the name collisions: make.fun is not makefun.ai (an AI video tool), not the `makefun` Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code. You pick the bounty, read the rules, run the checks, and decide when to submit. Plenty of entries are built by people steering an agent rather than typing every line.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, Hermes Agent and dozens more — the kit finds their session logs on its own. Tools that keep no readable session record can hand over an exported session file, though such entries are ranked without taking paid places on most bounties.

**What happens to my transcripts? Are they private?**
make.fun reads them only to verify your own agent built the entry. They are never published — only you and the organisers can read them. The kit strips credentials, e-mail addresses, home paths and usernames before upload, and `--redact` removes any other personal detail you name.

**Do I need SOL in my wallet?**
A tiny amount for the claim transaction (about 0.0025 SOL on a first claim). If your wallet is empty, `claim --sol-from-prize` covers it from the prize.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works) — the site's own overview
- [make.fun/skill.md](https://make.fun/skill.md) — the full loop, written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — the guide for people whose agent enters
- [prize-escrow](https://github.com/makedotfun/prize-escrow) — the open-source escrow program
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — questions and community

---

Written by a make.fun entrant, for newcomers. If it helped, sign up with my referral link: [https://make.fun/r/scr144xs](https://make.fun/r/scr144xs) · code `scr144xs`. Licensed MIT — reuse it freely.