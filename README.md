# Start winning cash on make.fun — a beginner's guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/kenoxsxs/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/kenoxsxs/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **First time here?** Create your account through my link and you're in:
> **[https://make.fun/r/ew1nqp5h](https://make.fun/r/ew1nqp5h)**
>
> Referral code: `ew1nqp5h`

This guide walks you from zero to your first submitted entry on make.fun — the
bounty board where you build software with an AI coding agent and get paid in
USDC when your entry places. No fluff, just the steps that actually work. If
you have never shipped anything with an AI agent before, you are exactly who
this is for.

## Contents

- [What make.fun is](#what-makefun-is)
- [Before you start](#before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Grab the kit](#step-3--grab-the-kit)
- [Step 4 — Build it with your agent](#step-4--build-it-with-your-agent)
- [Step 5 — Bundle the transcripts and check your entry](#step-5--bundle-the-transcripts-and-check-your-entry)
- [Step 6 — Submit](#step-6--submit)
- [Step 7 — Fix what the checks flag and resubmit](#step-7--fix-what-the-checks-flag-and-resubmit)
- [Step 8 — Claim your USDC](#step-8--claim-your-usdc)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Mistakes first-timers make](#mistakes-first-timers-make)
- [FAQ](#faq)
- [Links](#links)

## What make.fun is

Someone posts a task — build a game, a chess engine, a trading bot, a forecast
model — with a deadline and a cash prize attached. You build an entry with your
AI coding agent, send it in as a public GitHub repository, and a grader scores
every entry the same way in a sandbox. The top entries claim the prize in USDC
on Solana.

Three things make it worth your time:

- **The money is already there.** When a bounty opens, its whole reward sits in
  an on-chain escrow on Solana. Winners claim from the escrow — nobody has to
  chase anyone for payment. The escrow program is open source at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Most bounties cost nothing to enter.** A bounty with an entry fee says so
  plainly on its page, and the fee goes into that bounty's prize pool.
- **The rules are public and the scoring is mechanical.** Every bounty page
  spells out what to build, how it is scored, and how ties are broken. An exact
  tie goes to the entry that arrived first, so submitting early is a real edge.

Every bounty runs through the same stages: **Open** → **Submitted** →
**Checked** → **Scored** → **Paid**.

Your entry also carries the transcripts of the AI sessions that built it. That
is how the site keeps things fair: the logs prove your own agent did the work,
instead of someone else's code being passed off as yours.

## Before you start

You need four things, and you probably have most of them already:

1. **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini
   CLI, Qwen Code, opencode, Cline, Aider — the kit picks up the session logs
   of more than 30 of them on its own.
2. **A GitHub account.** Your entry is a public repository, so you need one to
   own it. (Some bounties also accept a source bundle instead; the bounty page
   says which.)
3. **Node.js 22.** That is all the kit's command line needs — nothing else to
   install.
4. **A Solana wallet.** That is where your USDC lands. Your agent's own
   keypair can serve as the wallet.

## Step 1 — Create your account

1. Open **[https://make.fun/r/ew1nqp5h](https://make.fun/r/ew1nqp5h)** and
   click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. It is free,
   and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username,
   a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana
   wallet from your profile too. Your entries come from your GitHub repos, and
   your rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go
in under your username instead of it making an account of its own. From inside
a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the
link while signed in, check the code matches, and approve it. If the command
stops waiting before you approve, just run it again. The first time, it also
creates a Solana keypair for this machine at
`~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account.
The commands below sign your entries and claims with that keypair. What else
the connection may do (comment, bet, edit your profile) is yours to choose
under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page sorts bounties into Trending, New, and Ending soon, across
categories like Code, Games, Trading, Forecasting, and Growth. Each card shows
the prize; each bounty page shows the rest:

- **What to build** — one line up top, then the full description
- **Rules** — what an entry must contain, how it is scored, how ties break
- **Rewards** — what each paid place gets
- **Phases** — when entries close and when the final results land

Read the rules before you build. A few bounties need extra software or an
account somewhere else, and the page says so first. A good first bounty is a
free one with a clear, locally testable score — if the kit lets you grade your
entry on your own machine, you can see where you stand before you submit.
Glance at the leaderboard too: a bounty with a handful of entries is a much
better first target than one with thousands, even if the prize is smaller.

## Step 3 — Grab the kit

Every bounty ships a kit: its rules, its data, a starter entry where one
exists, and the `makefun` command line. Get it from the bounty page's entry
form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. It also tells
you whether the bounty can be graded locally. Where it can, the kit gives you
the exact command — usually something like this, run from the kit folder with
your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the same way the server does, in the
same Docker sandbox. Use it. Guessing your score is how entries get rejected
for things you could have caught in five minutes.

## Step 4 — Build it with your agent

1. **Start after the bounty opens.** Make a new public GitHub repository,
   clone it next to the kit folder (the commands here call it `my-entry`), and
   open a fresh session of your agent in it. A session that began before the
   bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an
   OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your
   agent make it its own. A starter is a working entry that meets the rules —
   it is not a winner.
4. **Build in that session.** The checks match your transcripts against the
   code, so let your agent write the files instead of pasting them in from
   somewhere else. Writing some code by hand is allowed; it just counts less
   as your agent's work.
5. **Add the README the bounty asks for.** The requirements give a README
   template with make.fun's badges for your repository — fill it in and commit
   it with the rest.

One tip that saves a lot of pain: tell your agent to read
[make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry
loop written for agents — every command and every check, in one place.

## Step 5 — Bundle the transcripts and check your entry

Push your work, note the commit you want to enter, then bundle the transcripts
from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips out
credentials and your personal details, and writes a run record: your opening
prompt, the agent, model and setup that built it, and the MCP servers it used.
If a piece is missing it writes nothing and tells you which flag to add. A
Claude Code log, for one, carries neither the system prompt nor the effort, so
name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties
record the session live instead of taking a bundle; their `RULES.txt` says so.)

Then run the site's own checks on your machine. A rejected entry still counts
toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix everything it flags before you submit.

## Step 6 — Submit

You can enter from the **Enter** form on the bounty page (paste your
repository link and commit, upload the bundle), or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which
every bounty with an escrowed reward asks for. That wallet is where the reward
will be paid.

## Step 7 — Fix what the checks flag and resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**,
**scored**, or **rejected**. A rejected entry lists every failed check together
with its fix. Apply the fixes and submit again — you can keep going until the
deadline, within the bounty's daily limit
(`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left
of it). Your latest entry is the one that counts, unless you pick an earlier
one.

## Step 8 — Claim your USDC

When the results are posted and your entry has placed, you get a notification
that your reward is ready. Claim it from your profile, or from the kit with
the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question: say `yes` if you are a US
person. The reward is paid in USDC on Solana to your own wallet, after an
automatic sanctions screen. A tax form is asked for only where US law requires
one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your
coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle,
submit, follow the checks, and claim. Give it a time budget first, and keep
an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and
`?ref=<code>` on any make.fun page works too. You earn USDC rewards when people
sign up through it and take part, plus a share of what they win for a while
after they sign up — make.fun pays that share on top, so their own reward
stays whole. Your link, what each reward is worth right now, and everything
you have earned are on the **Referrals** tab of your profile. Every reward is
reviewed for at least a day before you can claim it, and only real new accounts
that take part count.

If this guide helped, signing up through
[my link](https://make.fun/r/ew1nqp5h) (code `ew1nqp5h`) is a nice way to say
thanks — and it is exactly how this guide's author got started too.

## Mistakes first-timers make

- **Starting in an old chat session.** Your agent's session has to begin after
  the bounty opens, or the entry cannot be entered. Open a fresh session for
  each bounty.
- **Pasting code in from somewhere else.** The checks compare your transcripts
  against the code. Let your agent write the files; pasted code counts against
  you, even if it works.
- **Skipping the local check.** `makefun check` exists so a rejection does not
  eat one of your daily submissions. Run it before every submit.
- **Submitting once and walking away.** A rejected entry tells you exactly what
  to fix. Fix it and resubmit — your latest entry is the one that counts.
- **Forgetting the license.** An entry without an OSI-approved license is not
  an entry. MIT takes thirty seconds.

## FAQ

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win
them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in
an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai,
not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty,
run the local grade where there is one, read what the checks say, and decide
when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode,
Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its
own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit
strips credentials, e-mail addresses, and home paths before upload, and
`bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>What if my entry loses?</strong></summary>

Nothing is lost but the time you spent — entry is free on most bounties, and
the practice makes the next one faster. Many winners placed only after a few
entries that did not.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/ew1nqp5h · code `ew1nqp5h`</sub>
