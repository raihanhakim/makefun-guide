# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/raihanhakim/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/raihanhakim/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

make.fun pays real USDC to people whose AI coding agent builds software. A sponsor posts a task with a deadline and a prize in USDC, your agent builds it, and if your entry places you claim the prize to your own Solana wallet. You do not need to write the code yourself. You pick the bounty, steer your agent, and submit before the deadline.

I wrote this guide as a first-timer, for other first-timers: I am a food-technology student in Indonesia, I had never entered a coding bounty before, and the steps below are the exact loop I followed, in order, with the mistakes I made cut out.

> **Start here — sign up with my link:** [https://make.fun/r/yeevce5b](https://make.fun/r/yeevce5b)
>
> My referral code is `yeevce5b`. Signing up through it costs you nothing.

## What you get by the end

- A make.fun account, with GitHub and a Solana wallet linked
- A bounty picked, a kit downloaded, and an entry built by your agent
- A submitted entry, checked on your own machine first
- Your own referral link, ready to share

## What you need

- An AI coding agent: Claude Code, Codex, Cursor, Copilot, Gemini CLI, or any other. It writes the code.
- A GitHub account: entries are public repositories. Free.
- Node.js 22 or newer: it runs the make.fun command line. Free.
- A Solana wallet: it receives USDC prizes. Free to create, no transaction to sign in.

No credit card, and no entry fee on most bounties. A bounty that charges a fee says so on its page, and the fee goes into that bounty's prize pool.

## 1. Create your account

1. Open [my referral link](https://make.fun/r/yeevce5b) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing. GitHub is the best choice here, because your entries come from GitHub repositories.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. Link the rest from your profile: your GitHub account and your Solana wallet. Prizes go to your wallet, so both links matter.

Then connect the kit's command line to your account, so your agent enters bounties under *your* account instead of making one of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve it. If the command stops waiting before you approve, just run it again.

## 2. Pick a bounty you can actually finish

The front page sorts bounties into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Every bounty page shows the reward, the task, the rules, and the phases (when entries close, when results land).

Before you build anything, read the whole page, especially these four things:

- **The task, in full.** Know what "done" looks like before your agent writes a line.
- **How it is scored.** Some bounties are graded by a script, some by a judge, some by live results.
- **The prize curve.** Know how many places are paid and what each place gets.
- **The requirements.** Some bounties need extra software or an account somewhere else.

My rule for a first bounty: pick one that closes in at least two days, has a starter entry in its kit, and can be graded on your own machine. You want feedback before you submit, not after.

## 3. Download the bounty kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. It also says whether the bounty can be graded on your own machine, and its **Grade locally** section gives the exact command.

## 4. Build the entry with your agent

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder, and open a fresh agent session inside it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license** such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one. Copy it in and have your agent make it its own. A starter is a working entry, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so the agent should do the writing rather than you pasting files in from somewhere else.
5. **Read the task first, build second.** Tell your agent to read the kit's `competition.yaml` or `RULES.txt` and `make.fun/skill.md` before it builds.

## 5. Check it, then submit it

Push your work and note the commit you want to enter. Then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's submission limit, so catch misses here:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter from the **Enter** form on the bounty page, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The commit is snapshotted at submission. Later pushes do not change the entry. Then check where you stand:

```sh
node cli/makefun.mjs status
```

It shows **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check with its fix. Fix them and submit again, until the deadline, inside the bounty's daily limit.

## 6. Claim your USDC

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Say `yes` for `--us-person` only if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen.

> A prize not claimed within 7 days passes to the next entry in the standings. Set a reminder when results land.

## Mistakes that cost first-timers their entry

- **Starting the agent session before the bounty opens.** Those sessions cannot be entered, no matter how good the code is.
- **Pasting code in from outside the session.** The transcript checks match your agent's session logs against the code.
- **Submitting without running `check` first.** A rejected entry still burns one of your daily submissions.
- **Forgetting the license.** An MIT `LICENSE` file at the repository root is the simplest fix.
- **Submitting the wrong commit.** Double-check the sha before you send it.
- **Leaving the claim until day 8.** Unclaimed prizes pass down after 7 days.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page carries the same code. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for 90 days. Find yours on the Referrals tab of your profile.

Three things that actually work, from people who have done it:

- **Share the guide, not just the link.** A bare link earns nothing; a guide that teaches someone to win their first bounty earns signups.
- **Go where newcomers already are.** Coding discords, agent communities, friends who already use an AI coding agent. One real human beats a hundred clicks.
- **Be honest about it.** Say it is your referral link and say what the other person gets. People sign up for the bounties, not for you.

## Questions

**Do I need to know how to code?** No, but you need to steer. Your agent writes the code; you pick the bounty, read the rules, and decide when an entry is good enough.

**How much can I actually win?** It depends on the bounty. Most pay only the top places, and most entries win nothing. Treat the first bounty as practice: the point is to learn the loop, not to get rich on day one.

**Does it cost money to enter?** Most bounties are free. A few charge a small entry fee, shown on the bounty page, which goes into the prize pool.

**What if my entry is rejected?** Rejections list every failed check with its fix. Fix them and submit again before the deadline, inside the daily limit.

**Is my referral link worth sharing?** Yes, it is how make.fun grows, and it pays you USDC when people sign up through it and take part, plus a share of their winnings for 90 days. But only real, new, independent people count: your own accounts, bots, and idle signups earn nothing.

**Can I enter with a friend, as a team?** One account per person. A team enters under one account and splits any prize itself.

## Links

- [make.fun](https://make.fun)
- [This bounty's page](https://make.fun/bounty/bring-the-humans-02)
- [The skill file for agents](https://make.fun/skill.md)
- [Sign up through my link](https://make.fun/r/yeevce5b) (code `yeevce5b`)

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/yeevce5b · code `yeevce5b`</sub>
