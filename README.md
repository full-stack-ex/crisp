# Crisp

Tired of your AI assistant talking to you like a mad scientist? Crisp is an Agent Skill that makes it talk like a competent colleague instead.

## What it does

Crisp puts the answer in the first sentence. Then it gets out of the way.

No "great question." No "let's dive in." No three-paragraph windup before the number you asked for.

And every reply that needs something from you ends with a numbered list of exactly what. No hunting through a wall of explanation for the one sentence that was an instruction.

## Assistant's responses before and after

Real example. The assistant had just finished a build and reported back:

**Before**

> Good news on the build. The compose file matches the reference exactly now, which confirms the earlier fixes landed correctly. There is one remaining difference in the Dockerfile: the helper functions we added to the docs last week are not present in your working image. This is expected, since you have not rebuilt since that change. The check is behaving correctly here and the difference is a real finding rather than a false positive. Adding those three lines to the bashrc section and rebuilding would bring everything into alignment, after which the verification should pass cleanly.

The reader would have to reverse-engineer the instruction out of the last sentence. Which three lines? Where? What command?

**After**

> Compose matches. The Dockerfile has one real difference, and it is the check working correctly.
> 
> Your image is missing the helper functions the docs added last week, because you have not rebuilt since.
> 
> **Action.** In WSL, paste this:
> 
> ```bash
> cd ~/project
> sed -i '/^myids$/i @xee () { cd "/workspace/${1:-}"; }' Dockerfile
> docker compose build dev
> ./check.sh
> ```
> 
> Expected: `PASS`. Paste me the output either way.

Same information. One is a status update you have to decode, the other is something you can act on in ten seconds.

A shorter one. Assistant reporting a scan result:

**Before**

> After running the analysis across the full document set, I found that the number of potentially problematic sections was quite high initially, though on closer inspection many of these turned out to be false positives rather than genuine issues.

**After**

> 93 candidates, 10 real. The other 83 were my detector missing four ways an expectation gets written.

## Install

Everything is in `SKILL.md`. That one file is the skill.

The easiest way: paste it into your assistant and ask it to install the skill for you.

To do it yourself, install the folder through your client's Agent Skills support.

Crisp uses the open [Agent Skills](https://agentskills.io/) format, has no scripts and no dependencies, so it drops in wherever your client loads skills from.

If your client has no skill support, `SKILL.md` works as persistent custom or system instructions.

## Usage

Once installed, most clients will apply Crisp on their own when a request matches the skill description. To be explicit:

```
Use /crisp to turn these findings into a short status report.
```

Invocation syntax varies by client ("/crisp", "@crisp", ...). Whatever yours uses for skills, use that.

## Tuning it

The rules are short on purpose. Open `SKILL.md`, change what bothers you, and keep the rest. It is a single plain-English file with no machinery behind it, so editing it directly is the intended way to use it.

Contribute your changes with a PR so everyone can benefit.
