# Assignment 2: Claim and Reproduce

**AI 301 | AI Open Source Capstone, Fall 2026**

This is the Unit 2 beat. You install and finish a grading skill, prove it against a scored test set, then use it on a real GitHub issue: post a claim, reproduce the bug, and post a repro report.

The standard for the report is: a stranger could re-run it, the evidence shows it, and you said what happened.

The deadline printed in the assignment is **Monday, September 28 at 2:59 AM EDT**. The assignment is worth **25 points**.

## What you do, in order

Steps 1–3 build and calibrate the skill. Steps 4–6 use it on your real issue. Step 7 submits.

1. **Install the skill.** Clone `https://github.com/codepath/ai301-unit2-starter.git` (or download the ZIP) and copy the whole `skill/` folder to `~/.claude/skills/repro-check/`. Claude finds skills under that path from any directory.

2. **Finish three files** in that installed copy, using the skill’s template formats: `rubric.md` (including your worksheet changes), `references/evidence-guide.md`, and `voice-guide.md`. Point the harness `--rubric` and `--evidence` flags at those same files so eval runs and live runs grade with the same judgment.

3. **Run the eval harness** from the clone’s `eval/` directory. Optional warm-up: grade `eval/packages/calib-02.md` by hand. Then run one full eval, read every disagreement with the gold labels, and re-run only those packages with `--only` while you revise. Repeat until you hit the bar: **18 of 20** scored packages agree with the gold labels, **and every category matches**. Then add `--save-run eval-run.txt` to one confirming full run. The harness writes that file; do not hand-edit it.

4. **Claim the issue.** Draft a claim comment, run the skill in live mode, revise until the verdict is accept, then post it. The claim comes before the reproduction: name the issue, say what you will do next, and promise the report. Do not assert that you already reproduced it.

5. **Fork Path Review.** Open the repo named in `scope.md`, fork it, and clone **your fork**. Later work (branch and pull request) comes from that fork. You cannot push to the shared repo. If Git asks for a password, use a GitHub personal access token.

6. **Reproduce the bug** in the sandbox environment described in the repo’s own docs. Write the repro report, run the skill in live mode on the full package (claim plus report), and post the repro comment when the skill says it is ready.

7. **Submit** a link to your **entire course repo** (the same Unit 1 repo), not a single folder, through the course portal.

## What goes in the course repo

Two repos are involved, and they do not mirror each other. The Unit 2 starter holds the skill and the harness; nothing you submit lives there. The course repo is where the graded work lands:

| Location | What belongs there |
|---|---|
| `tools/repro-check/` | Current skill files from `~/.claude/skills/repro-check/`: `SKILL.md`, `scope.md`, `rubric.md`, `references/evidence-guide.md`, `voice-guide.md` |
| `beat-1-sandbox/unit-2/reproduction.md` | Links plus pasted text of the claim and repro comments, short reflections, and four eval write-up fields |
| `beat-1-sandbox/unit-2/eval-run.txt` | One complete harness-written eval run |

**Points:** skill files **8**, eval run plus write-up **10**, posted comments **7**.

The 10 eval points are not the 18/20 tally. **3** are for committing one complete harness-written run (a partial `--limit` or `--only` run earns 0 and cannot be saved). **7** are split across four fields in `reproduction.md`, each scored on its own:

- Run history (2)
- Package analysis (2)
- Check rationale (2)
- Trade-offs (1)

A first run that already agrees on all 20 loses nothing. The score is for the account of how the tool reached that run.

The comment points want a claim that names the issue’s specifics, and a repro report a stranger could re-run, with the environment recorded and the observed behavior shown. Both are matched to you by the GitHub username in the phase file. An honest, evidenced “cannot reproduce” earns full points. A confident reproduction of the wrong behavior does not. Promise only the investigation, never a fix or a date.

## Retries and the 18-of-20 bar

Retries are unlimited before the deadline. A full eval run costs about **$4** of course credit. After that, re-grade only disagreements with `--only pkg-07,pkg-12` (about **$0.20** per package). If a revision loosens a check, add a canary package to that `--only` list before spending another full run. Check drafts in live mode before posting.

**Why the bar is 18 of 20:** 16 packages have loud signals a reasonable rubric should not miss; 4 are genuinely arguable. The 4 calibration packages from the activity are never scored. The category floor matters: the set contains exactly one disclosure-wall package, and a rubric with no conventions check cannot make up that miss by scoring well on volume.

## Key terms

**Skill.** A folder of instructions Claude Code loads automatically from `~/.claude/skills/`. Here the skill is `repro-check`: it grades a claim comment, and later a claim plus a repro report, against your rubric and guides.

**Rubric (`rubric.md`).** The checklist of pass/fail checks the skill applies. Your activity worksheet changes belong in this file. Eval and live runs must use this same file.

**Evidence guide (`references/evidence-guide.md`).** Rules for what counts as proof: what must be shown so a stranger could re-run the bug and see the same result.

**Voice guide (`voice-guide.md`).** Rules for how the comment should sound. This is where “promise the investigation, do not promise a fix or a date” is graded.

**Scope (`scope.md`).** The skill’s boundaries and the Path Review house rules, including which repo to fork.

**Harness.** The eval program in the starter repo’s `eval/` directory. It runs your skill against a fixed set of packages and writes `eval-run.txt` when you pass `--save-run` on a full run. The file header records the pinned model, the tool graded, and a fingerprint of each input file.

**Package.** One graded example the harness feeds the skill (a claim, or a claim plus a report). Scored packages have ids such as `pkg-07`. Calibration packages (`calib-02` and three others) are practice only and are never scored.

**Gold label.** The official verdict for a package. Agreement means your rubric’s verdict matches that label.

**Category / category floor.** Checks are grouped (for example, a conventions check). You must match every category, not only reach 18 of 20 overall. One miss in a category that appears only once cannot be offset by other packages.

**Disclosure-wall package.** The one package whose repo policy requires disclosing AI assistance. Comments that do not disclose fail it. A rubric with no conventions check cannot recover that miss.

**Canary.** When you loosen a check, you also re-grade one package from each single-package category that change could affect, so you notice if a package that used to agree now flips.

**Live mode.** Running the installed skill on your own draft, outside the harness:

```text
claude "repro-check: grade my draft claim comment in <file> for issue <URL>"
```

A real run reads your skill files, prints a short per-check summary, and ends with a fenced JSON block whose verdict is `accept` or `reject`. On a claim-only draft, the repro checks are reported as not yet applicable. If that JSON block is missing, the skill did not run.

**Claim comment.** The first GitHub comment on your issue. It names the issue and what you will do next, and it promises a repro report. It does not claim the bug is already reproduced. A classmate’s claim does not block you; post your own.

**Repro report / repro comment.** Your own write-up of trying to reproduce the bug: environment, steps, and observed behavior, in your own words. “Same as above, can confirm” does not count. Credit attaches to the work you post, not to being first.

**Path Review.** The shared class repo named in `scope.md`. You fork it, clone your fork, and do the reproduction there.

**Fork.** Your own GitHub copy of Path Review, under your account, which you can push to.

**Clone.** A local copy of a remote repo on your machine (`git clone <url>`).

**Sandbox repo.** The Path Review environment you set up from that repo’s own docs, then use to try to reproduce the bug.

**House issue.** A shared fallback issue. The same claim and repro criteria apply if you work there.

**Course repo.** The repo you created from the course template in Unit 1. That is what you submit. `tools/repro-check/` and `beat-1-sandbox/` are already in the template.

**Personal access token.** A GitHub credential used instead of your account password when `git push` asks you to authenticate.

**Fingerprint.** The harness’s record, inside `eval-run.txt`, of exactly which files were used, so the run can be tied to the rubric and guides you uploaded.
