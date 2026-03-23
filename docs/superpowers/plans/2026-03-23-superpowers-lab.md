# Superpowers Lab Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a disciplined experimentation workflow plugin for AI/ML research in Claude Code

**Architecture:** Plugin-based architecture with 8 composable skills, session-start hook for protocol injection, and file-based experiment tracking (LEDGER + individual plans + artifacts)

**Tech Stack:** Bash (hooks), Markdown (skills/docs), JSON (plugin manifest), Python integration examples (framework-agnostic)

---

## File Structure

```
superpowers-lab/
├── .claude-plugin/
│   └── plugin.json                          # Plugin manifest
├── skills/
│   ├── experiment-planning/
│   │   └── SKILL.md                         # Planning questionnaire + doc creation
│   ├── experiment-execution/
│   │   └── SKILL.md                         # Run tracking + artifact capture
│   ├── experiment-analysis/
│   │   └── SKILL.md                         # Comparison vs baseline + outcome labeling
│   ├── ablation-study/
│   │   └── SKILL.md                         # Systematic component removal
│   ├── hyperparameter-sweep/
│   │   └── SKILL.md                         # Grid/random search tracking
│   ├── baseline-comparison/
│   │   └── SKILL.md                         # Head-to-head model comparison
│   ├── reproducibility-check/
│   │   └── SKILL.md                         # Verify experiment can be recreated
│   └── verification-before-completion/
│       └── SKILL.md                         # Evidence before claims (reused pattern)
├── hooks/
│   └── session-start                        # Inject experimentation protocol
├── templates/
│   └── experiment-plan-template.md          # Template for new experiments
├── SKILLS/
│   └── experimentation-protocol.md          # Reference doc (injected by hook)
├── agents/
│   └── experiment-reviewer.md               # Optional: review experiment outcomes
├── docs/
│   └── superpowers/
│       ├── specs/
│       │   └── 2026-03-23-superpowers-lab-design.md
│       └── plans/
│           └── 2026-03-23-superpowers-lab.md
├── README.md
└── LICENSE
```

---

## Task 1: Project Foundation

**Files:**
- Create: `README.md`
- Create: `LICENSE`
- Create: `.gitignore`

- [ ] **Step 1: Create README.md**

```markdown
# Superpowers Lab

Disciplined experimentation workflow for AI/ML research in Claude Code.

## What it does

Superpowers Lab enforces the scientific method for ML experiments:
- Hypothesis → Plan → Execute → Analyze → Iterate
- Every experiment tracked with reproducibility metadata
- Systematic ablation studies and hyperparameter sweeps
- Evidence before claims - no false success

## Installation

```bash
# From local directory
/plugin install /path/to/superpowers-lab

# Coming soon to marketplace
/plugin install superpowers-lab
```

## Skills

- **experiment-planning** - Structured questionnaire → written plan
- **experiment-execution** - Run with reproducibility tracking
- **experiment-analysis** - Compare vs baseline, collect artifacts
- **ablation-study** - Systematic component removal
- **hyperparameter-sweep** - Grid/random search tracking
- **baseline-comparison** - Head-to-head model comparison
- **reproducibility-check** - Verify experiment can be recreated
- **verification-before-completion** - Evidence before claims

## Workflow

```
User: "I want to test if adding attention helps"
→ experiment-planning: Creates comprehensive plan
→ experiment-execution: Runs with tracking
→ experiment-analysis: Compares vs baseline
→ Suggests: Ablation study to confirm?
```

## File Structure

Projects get an `experiments/` directory:
```
experiments/
├── LEDGER.md                    # Quick overview
├── EXP-20260323-01.md           # Individual experiment
├── artifacts/                   # Checkpoints, logs, predictions
└── configs/                     # Reproducibility configs
```

## Philosophy

- No experiments without written plans
- Compare against real baselines
- One variable at a time (unless intentional multi-factor)
- Document everything (seed, data version, commit, command)
- INVALID label for buggy runs (don't count as evidence)

## License

MIT License - see LICENSE file
```

- [ ] **Step 2: Create LICENSE (MIT)**

```text
MIT License

Copyright (c) 2026 Superpowers Lab Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 3: Create .gitignore**

```gitignore
# Claude Code plugin
*.log
.DS_Store

# Python (for examples/tests)
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/

# Experiment artifacts (when testing locally)
experiments/artifacts/
experiments/configs/
!experiments/.gitkeep
```

- [ ] **Step 4: Commit foundation**

```bash
git add README.md LICENSE .gitignore
git commit -m "feat: add project foundation (README, LICENSE, gitignore)"
```

---

## Task 2: Plugin Manifest

**Files:**
- Create: `.claude-plugin/plugin.json`

- [ ] **Step 1: Create plugin manifest**

```json
{
  "name": "superpowers-lab",
  "version": "1.0.0",
  "description": "Disciplined experimentation workflow for AI/ML research",
  "author": "Superpowers Lab Contributors",
  "license": "MIT",
  "skills": [
    "skills/experiment-planning",
    "skills/experiment-execution",
    "skills/experiment-analysis",
    "skills/ablation-study",
    "skills/hyperparameter-sweep",
    "skills/baseline-comparison",
    "skills/reproducibility-check",
    "skills/verification-before-completion"
  ],
  "hooks": {
    "SessionStart": "hooks/session-start"
  }
}
```

- [ ] **Step 2: Commit plugin manifest**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: add Claude Code plugin manifest"
```

---

## Task 3: Experimentation Protocol Reference

**Files:**
- Create: `SKILLS/experimentation-protocol.md`

- [ ] **Step 1: Create protocol reference document**

```markdown
# Experimentation Protocol

This protocol is injected at session start via the session-start hook. All skills reference this document for the core methodology.

## Core Rules

1. **No experiments without a plan** - Every experiment must have a written plan document before execution
2. **Evidence before claims** - Never claim improvement without fresh verification
3. **Compare against real baselines** - Always have a baseline reference experiment
4. **One variable at a time** - Change one meaningful variable per experiment unless running a designed multi-factor study
5. **Document everything** - Command, seed, data version, commit hash, environment

## Experiment Lifecycle

1. **Plan** → Write experiment plan with hypothesis, baseline, acceptance criteria
2. **Execute** → Run with reproducibility tracking (seed, data version, config)
3. **Analyze** → Compare vs baseline, collect artifacts, assess confidence
4. **Decide** → WIN/LOSS/NEUTRAL/INVALID, document next action

## Required Metadata

Every experiment MUST record:
- Exact command run
- Config file + resolved hyperparameters
- Random seed(s)
- Dataset/version
- Code commit hash
- Results (baseline → new, delta)
- Outcome label + confidence

## Failure Modes

- **INVALID** = Bug, data leak, wrong config, crashed run
- Report INVALID clearly, state reason
- INVALID experiments don't count as evidence

## Outcome Labels

- **WIN** - Met acceptance criteria, improvement validated
- **LOSS** - Failed criteria, regression confirmed
- **NEUTRAL** - Inconclusive, needs more data
- **INVALID** - Bug, data leak, wrong config (state reason clearly)

## Confidence Levels

- **High** - Clear result, multiple runs, statistical significance
- **Medium** - Direction clear but magnitude uncertain
- **Low** - Too noisy, needs more data
```

- [ ] **Step 2: Commit protocol reference**

```bash
git add SKILLS/experimentation-protocol.md
git commit -m "feat: add experimentation protocol reference document"
```

---

## Task 4: Session-Start Hook

**Files:**
- Create: `hooks/session-start`

- [ ] **Step 1: Create session-start hook**

```bash
#!/usr/bin/env bash
# session-start - Inject experimentation protocol at session start

cat << 'EXPERIMENT_PROTOCOL'
# Experimentation Protocol (Superpowers Lab)

## Core Rules

1. **No experiments without a plan** - Every experiment must have a written plan document before execution
2. **Evidence before claims** - Never claim improvement without fresh verification
3. **Compare against real baselines** - Always have a baseline reference experiment
4. **One variable at a time** - Change one meaningful variable per experiment unless running a designed multi-factor study
5. **Document everything** - Command, seed, data version, commit hash, environment

## Experiment Lifecycle

1. **Plan** → Write experiment plan with hypothesis, baseline, acceptance criteria
2. **Execute** → Run with reproducibility tracking (seed, data version, config)
3. **Analyze** → Compare vs baseline, collect artifacts, assess confidence
4. **Decide** → WIN/LOSS/NEUTRAL/INVALID, document next action

## Required Metadata

Every experiment MUST record:
- Exact command run
- Config file + resolved hyperparameters
- Random seed(s)
- Dataset/version
- Code commit hash
- Results (baseline → new, delta)
- Outcome label + confidence

## Failure Modes

- **INVALID** = Bug, data leak, wrong config, crashed run
- Report INVALID clearly, state reason
- INVALID experiments don't count as evidence

@SKILLS/experimentation-protocol.md
EXPERIMENT_PROTOCOL
```

- [ ] **Step 2: Make hook executable**

```bash
chmod +x hooks/session-start
```

- [ ] **Step 3: Test hook locally (manual verification)**

Run: `./hooks/session-start`
Expected: Outputs the protocol text to stdout

- [ ] **Step 4: Commit hook**

```bash
git add hooks/session-start
git commit -m "feat: add session-start hook for protocol injection"
```

---

## Task 5: Experiment Plan Template

**Files:**
- Create: `templates/experiment-plan-template.md`

- [ ] **Step 1: Create experiment plan template**

```markdown
# EXP-YYYYMMDD-XX — <short title>

## Metadata
- **Date/Time:**
- **Owner:**
- **Status:** Planned | Running | Completed | Failed

## Objective
- What are we testing?

## Hypothesis
- What do we believe will happen?

## Baseline Reference
- **Experiment ID:** EXP-YYYYMMDD-YY
- **Metric:** 82.3% accuracy

## Dataset
- **Version/Split:**
- **Train/Val/Test sizes:**

## Change(s) from Baseline
-

## Acceptance Criteria
- **Primary metric:** target ≥ 84.0%
- **Secondary metrics:**

## Risk Assessment
- **What could go wrong:**
- **Time estimate:**
- **Compute cost:**

## Resource Requirements
- **GPU memory:**
- **Estimated runtime:**

## Success/Failure Criteria
- **Stop if:**
- **Pivot if:**

## Reproducibility
- **Config path:**
- **Exact command:**
- **Seed(s):**
- **Code commit hash:**

## Results (filled after execution)
- **Primary metric:** <baseline> → <new> (delta: <+/->)
- **Secondary metrics:**
- **Runtime/Cost:**

## Outcome
- **Label:** WIN | LOSS | NEUTRAL | INVALID
- **What worked:**
- **What didn't work:**
- **Why (explanation):**
- **Confidence:** Low | Medium | High

## Artifacts
- **Checkpoints:**
- **Logs:**
- **Predictions:**

## Next Action
-
```

- [ ] **Step 2: Commit template**

```bash
git add templates/experiment-plan-template.md
git commit -m "feat: add experiment plan template"
```

---

## Task 6: Core Skill - Experiment Planning

**Files:**
- Create: `skills/experiment-planning/SKILL.md`

- [ ] **Step 1: Create experiment-planning skill**

```markdown
# Experiment Planning

Transform rough experiment ideas into comprehensive written plans.

## When To Use

**Triggers:**
- "run an experiment"
- "test a hypothesis"
- "design an experiment"
- "try X and see"
- "what happens if"

**Use when:** User mentions experiment-related activities before having a written plan.

**Don't use when:** User already has a written plan document.

## The Process

### 1. Detect Intent

User mentions experiment, hypothesis, testing → invoke this skill.

### 2. Administer Questionnaire

Ask questions one at a time. Build the plan iteratively.

**Required fields:**

**Objective:**
- "What are you trying to learn from this experiment?"

**Hypothesis:**
- "What do you believe will happen?"
- "Why do you believe this?"

**Baseline Reference:**
- "What's the baseline experiment ID? (or 'none' for first experiment)"
- "What's the baseline metric?"

**Dataset:**
- "What dataset version/split?"
- "Train/val/test sizes?"

**Change(s) from Baseline:**
- "What are you changing compared to baseline?"
- "Be specific: architecture, hyperparameters, data, etc."

**Acceptance Criteria:**
- "What's the target improvement?"
- "Primary metric to optimize?"
- "Secondary metrics to track?"

**Risk Assessment:**
- "What could go wrong?"
- "Time estimate?"
- "Compute cost estimate?"

**Resource Requirements:**
- "GPU memory needed?"
- "Estimated runtime?"

**Success/Failure Criteria:**
- "When should we stop early?" (overfitting, no improvement, etc.)
- "When should we pivot?" (wrong direction, better alternative, etc.)

**Reproducibility:**
- "Config file path?"
- "Exact command to run?"
- "Random seed(s)?"
- "Current git commit hash?"

### 3. Generate Experiment ID

Format: `EXP-YYYYMMDD-XX`

Use today's date, increment suffix for today's experiments (01, 02, 03...).

### 4. Create Experiment Plan Document

Use template from `templates/experiment-plan-template.md`.

Fill in all fields from questionnaire.

Save to: `experiments/EXP-YYYYMMDD-XX.md`

### 5. Update LEDGER

Add entry to `experiments/LEDGER.md`:

```markdown
| EXP-YYYYMMDD-XX | YYYY-MM-DD | <hypothesis summary> | Planned | - | - |
```

### 6. Summarize and Invoke Next Skill

**Example output:**

```
Experiment plan created: EXP-20260323-01

Hypothesis: Adding attention improves sequence modeling
Baseline: 82.3% accuracy
Target: ≥84.0% (acceptance criteria)
Risk: May increase training time 20-30%
Resources: 1x A100, ~2 hours

Plan saved to: experiments/EXP-20260323-01.md
LEDGER updated

Ready to execute. Say "run the experiment" to begin.
```

**Auto-invoke:** `experiment-execution` skill after user confirms.

## Common Mistakes

**❌ Skipping baseline:**
- First experiment still needs baseline metric (current best known result)

**❌ Vague hypotheses:**
- "Make it better" → "Adding attention layer will improve accuracy by 1-5%"

**❌ No acceptance criteria:**
- How will you know if it worked?

**❌ Missing reproducibility info:**
- Command, seed, data version, commit hash are mandatory

## Example Dialogue

```
User: "I want to test if adding attention helps my sequence model"

Agent: "I'll help you design an experiment. Let me ask some questions...

[Questionnaire: objective, hypothesis, baseline, metrics, etc.]

Agent: "Creating experiment plan EXP-20260323-01...
Hypothesis: Adding attention improves sequence modeling accuracy
Baseline: EXP-20260320-05 (82.3%)
Target: ≥84.0% accuracy
Acceptance: +1.7% improvement
Risk: 20-30% slower training
Resources: A100, ~2 hours
Seed: 42, data v3, commit abc123

Plan saved to: experiments/EXP-20260323-01.md
LEDGER updated

Say 'run the experiment' to begin."
```

## References

@SKILLS/experimentation-protocol.md
@templates/experiment-plan-template.md
```

- [ ] **Step 2: Commit experiment-planning skill**

```bash
git add skills/experiment-planning/SKILL.md
git commit -m "feat: add experiment-planning skill"
```

---

## Task 7: Core Skill - Experiment Execution

**Files:**
- Create: `skills/experiment-execution/SKILL.md`

- [ ] **Step 1: Create experiment-execution skill**

```markdown
# Experiment Execution

Run experiments with full reproducibility tracking and artifact collection.

## When To Use

**Triggers:**
- "run the experiment"
- "start training"
- "launch the experiment"
- "execute EXP-..."
- Auto-invoked after `experiment-planning` user approval

**Use when:** User has an approved experiment plan and wants to run it.

**Don't use when:** No experiment plan exists yet (use `experiment-planning` first).

## The Process

### 1. Load Experiment Plan

Read from: `experiments/EXP-YYYYMMDD-XX.md`

Verify:
- Plan exists
- All required fields are filled
- Reproducibility metadata is present

If missing: "Plan incomplete. Please complete [missing field] first."

### 2. Pre-Execution Checklist

**Verify:**
- [ ] Config file exists at specified path
- [ ] Dataset is available at specified version
- [ ] Git commit hash matches current HEAD (or warn user)
- [ ] Command is complete and executable

**Notify user:**
```
Running EXP-20260323-01

Tracking:
- Seed: 42
- Data: v3
- Config: experiments/configs/EXP-20260323-01-config.yaml
- Commit: abc123

Command: python train.py --config experiments/configs/EXP-20260323-01-config.yaml
```

### 3. Execute Experiment

**Run the exact command from the plan.**

Monitor output in real-time. Capture:
- stdout/stderr
- Training progress
- Any errors or warnings

### 4. Update Status

Update experiment plan status: `Planned` → `Running`

Update LEDGER status: `Planned` → `Running`

### 5. Monitor Execution

While experiment runs:
- Display progress to user
- Note any anomalies (errors, warnings, unexpected behavior)

### 6. Post-Execution

**On success:**
1. Capture final metrics from output
2. Save run info to `experiments/artifacts/EXP-XXXX/run-info.txt`:
   ```
   Command: <exact command>
   Seed: <seed>
   Data version: <version>
   Commit: <hash>
   Environment: <python version, packages>
   Start time: <timestamp>
   End time: <timestamp>
   Duration: <hours>
   ```
3. Update experiment plan with Results section
4. Update status: `Running` → `Completed`
5. Notify user: "Experiment finished. Analyzing results vs baseline..."
6. **Auto-invoke:** `experiment-analysis` skill

**On failure:**
1. Capture error output
2. Update experiment plan with error details
3. Update status: `Running` → `Failed`
4. Determine if INVALID (bug, wrong config) or legitimate failure
5. Notify user with diagnosis
6. Suggest next action (fix and retry, pivot, etc.)

## Artifact Collection

After successful execution, ensure artifacts are saved:

```
experiments/artifacts/EXP-XXXX/
├── checkpoints/          # Model checkpoints
├── logs/                 # Training logs
├── predictions/          # Test set predictions
├── plots/                # Metrics plots
├── metrics.json          # Parsed metrics
└── run-info.txt          # Command, seed, environment
```

**Note:** The skill should remind the user to save artifacts if not automatically done by the training script.

## Common Mistakes

**❌ Running without plan:**
- "No experiment plan found. Use experiment-planning first."

**❌ Wrong git commit:**
- Warn user: "Commit hash doesn't match. Current: def456, Plan: abc123. Proceed anyway?"

**❌ Missing artifacts:**
- Remind user to save checkpoints, logs, predictions

**❌ Not capturing command output:**
- Must save stdout/stderr for reproducibility

## Example Dialogue

```
User: "run the experiment"

Agent: "Running EXP-20260323-01

Tracking:
- Seed: 42
- Data: v3
- Config: experiments/configs/EXP-20260323-01-config.yaml
- Commit: abc123

Command: python train.py --config experiments/configs/EXP-20260323-01-config.yaml
[Monitors training output...]

Experiment finished. I'll now analyze results vs baseline..."
```

## References

@SKILLS/experimentation-protocol.md
@skills/experiment-planning/SKILL.md
@skills/experiment-analysis/SKILL.md
```

- [ ] **Step 2: Commit experiment-execution skill**

```bash
git add skills/experiment-execution/SKILL.md
git commit -m "feat: add experiment-execution skill"
```

---

## Task 8: Core Skill - Experiment Analysis

**Files:**
- Create: `skills/experiment-analysis/SKILL.md`

- [ ] **Step 1: Create experiment-analysis skill**

```markdown
# Experiment Analysis

Compare results vs baseline, collect artifacts, assess outcome (WIN/LOSS/NEUTRAL/INVALID).

## When To Use

**Triggers:**
- "analyze results"
- "compare with baseline"
- "what did we learn"
- Auto-invoked after `experiment-execution` completes

**Use when:** Experiment has finished running and results need to be interpreted.

**Don't use when:** Experiment hasn't run yet or is still running.

## The Process

### 1. Load Documents

Load:
- Current experiment: `experiments/EXP-YYYYMMDD-XX.md`
- Baseline experiment: `experiments/EXP-YYYYMMDD-YY.md` (if exists)

### 2. Extract Metrics

From Results section of experiment plan:
- Primary metric value
- Secondary metrics
- Runtime, cost

From baseline:
- Baseline metric values

### 3. Calculate Deltas

For each metric:
- Absolute delta: `new - baseline`
- Relative delta: `((new - baseline) / baseline) * 100`

Example:
```
Accuracy: 82.3% → 85.5% (+3.2%, +3.9%)
Loss: 0.45 → 0.38 (-0.07, -15.6%)
Runtime: 1.2h → 1.5h (+0.3h, +25%)
```

### 4. Compare Against Acceptance Criteria

From experiment plan's Acceptance Criteria section:
- Primary metric: Was target met?
- Secondary metrics: Any red flags?

**Determine outcome:**

| Condition | Outcome |
|-----------|---------|
| Met acceptance criteria, improvement validated | WIN |
| Failed criteria, regression confirmed | LOSS |
| Inconclusive, needs more data | NEUTRAL |
| Bug, data leak, wrong config, crashed | INVALID |

### 5. Assess Confidence

**High:**
- Clear improvement/degradation
- Multiple runs consistent
- Statistical significance (if applicable)

**Medium:**
- Direction clear but magnitude uncertain
- Single run only

**Low:**
- Too noisy
- Conflicting metrics
- Needs more data

### 6. Collect Artifacts

Ensure artifacts are saved to `experiments/artifacts/EXP-XXXX/`:
- Checkpoints
- Logs
- Predictions
- Metrics JSON
- Run info

If missing: "Please save [missing artifact] to experiments/artifacts/EXP-XXXX/"

### 7. Update Experiment Plan

Fill in Results and Outcome sections:

```markdown
## Results
- **Primary metric:** 82.3% → 85.5% (+3.2%)
- **Secondary metrics:** Loss: 0.45 → 0.38 (-15.6%)
- **Runtime/Cost:** 1.2h → 1.5h (+25%)

## Outcome
- **Label:** WIN
- **What worked:** Adding attention improved accuracy as expected
- **What didn't work:** Training time increased more than expected
- **Why:** Attention allows better long-range dependencies
- **Confidence:** High
```

### 8. Update LEDGER

Update entry in `experiments/LEDGER.md`:

```markdown
| EXP-20260323-01 | 2026-03-23 | Attention improves seq modeling | WIN | 82.3% → 85.5% (+3.2%) | High |
```

### 9. Suggest Next Action

Based on outcome:

**WIN:**
- "Results improved by X%. Should we run an ablation study to confirm which component helped?"
- "Should we try further improvements?"

**LOSS:**
- "Results regressed by X. Should we pivot to different approach?"
- "Should we analyze why it didn't work?"

**NEUTRAL:**
- "Results inconclusive. Should we run with more data/different seed?"

**INVALID:**
- "Experiment was INVALID due to [reason]. Fix and retry?"

## Common Mistakes

**❌ Missing baseline:**
- First experiment: "No baseline to compare. This is the new baseline."

**❌ Ignoring secondary metrics:**
- Primary improved but loss exploded → might be overfitting

**❌ Wrong outcome label:**
- INVALID doesn't mean "didn't work" - it means "buggy run"

**❌ No next action:**
- Always suggest what to do next

## Example Dialogue

```
User: [after experiment finishes]

Agent: "Analyzing results vs baseline...

Results:
- Accuracy: 82.3% → 85.5% (+3.2%, +3.9%)
- Loss: 0.45 → 0.38 (-15.6%)
- Runtime: 1.2h → 1.5h (+25%)

Acceptance criteria: ≥84.0% accuracy → MET (85.5%)

Outcome: WIN, Confidence: High
What worked: Adding attention improved accuracy
What didn't: Training time increased more than expected
Why: Attention captures long-range dependencies better

Artifacts saved to: experiments/artifacts/EXP-20260323-01/
LEDGER updated

Should we run an ablation study to confirm the attention layer is responsible?"
```

## References

@SKILLS/experimentation-protocol.md
@skills/experiment-execution/SKILL.md
```

- [ ] **Step 2: Commit experiment-analysis skill**

```bash
git add skills/experiment-analysis/SKILL.md
git commit -m "feat: add experiment-analysis skill"
```

---

## Task 9: Specialized Skill - Ablation Study

**Files:**
- Create: `skills/ablation-study/SKILL.md`

- [ ] **Step 1: Create ablation-study skill**

```markdown
# Ablation Study

Systematically remove or vary components to isolate what contributes to performance.

## When To Use

**Triggers:**
- "ablation"
- "what if we remove X"
- "which component helped"
- "isolate the improvement"

**Use when:** User wants to understand which components of a model/config are responsible for performance.

**Don't use when:** No baseline experiment exists to ablate from.

## The Process

### 1. Identify Base Experiment

Load the "full" experiment to ablate: `experiments/EXP-YYYYMMDD-XX.md`

### 2. Identify Components to Ablate

**From user:**
- "Which components should we ablate?"

**Or suggest from config:**
- "I see these components in the config: [attention, layer_norm, dropout]. Which should we ablate?"

### 3. Design Ablation Matrix

For each component, create an experiment that removes/disables it.

Example:
```
Base: Full model (attention + layer_norm + dropout)
Ablation 1: Remove attention
Ablation 2: Remove layer_norm
Ablation 3: Remove dropout
```

### 4. Create Ablation Experiments

For each ablation:
1. Create child experiment plan using `experiment-planning` skill
2. Use same seed, data, commit as base
3. Only change: the component being ablated
4. Experiment ID: `EXP-YYYYMMDD-XX-ablate-<component>`

### 5. Execute Ablation Experiments

For each ablation experiment:
1. Run using `experiment-execution` skill
2. Analyze using `experiment-analysis` skill

### 6. Generate Comparison Report

Create ablation study report:

```markdown
# Ablation Study: EXP-YYYYMMDD-XX

## Base Model
- **Metric:** 85.5% accuracy
- **Components:** attention, layer_norm, dropout

## Ablation Results

| Component Removed | Accuracy | Delta | Conclusion |
|-------------------|----------|-------|------------|
| None (base) | 85.5% | - | - |
| Attention | 82.1% | -3.4% | Attention critical |
| Layer Norm | 84.8% | -0.7% | Minor contribution |
| Dropout | 85.8% | +0.3% | Dropout not needed |

## Recommendations

- **Keep:** Attention (major contribution)
- **Keep:** Layer Norm (small but positive)
- **Remove:** Dropout (hurts performance)
```

Save to: `experiments/EXP-YYYYMMDD-XX-ablation-study.md`

### 7. Update Base Experiment

Link to ablation study from base experiment plan:

```markdown
## Related Experiments
- Ablation study: [EXP-YYYYMMDD-XX-ablation-study](./EXP-YYYYMMDD-XX-ablation-study.md)
```

## Common Mistakes

**❌ Changing multiple variables:**
- Each ablation should only change one component

**❌ Different seed/data:**
- Must use same seed, data, commit for fair comparison

**❌ Not documenting what was removed:**
- Clearly state which component was ablated

## Example Dialogue

```
User: "Run an ablation study on EXP-20260323-01"

Agent: "I'll ablate the components from EXP-20260323-01.

Components found: attention, layer_norm, dropout

Ablation plan:
1. Remove attention
2. Remove layer_norm
3. Remove dropout

Each will use same seed (42), data (v3), commit (abc123).

Creating ablation experiments...
[Creates 3 child experiments]

Running ablation 1/3: Remove attention...
[Runs and analyzes...]

Running ablation 2/3: Remove layer_norm...
[Runs and analyzes...]

Running ablation 3/3: Remove dropout...
[Runs and analyzes...]

Ablation study complete!

Base: 85.5% (attention + layer_norm + dropout)
- Remove attention: 82.1% (-3.4%) ← Critical
- Remove layer_norm: 84.8% (-0.7%) ← Minor
- Remove dropout: 85.8% (+0.3%) ← Not needed

Recommendation: Keep attention and layer_norm, remove dropout.

Report saved to: experiments/EXP-20260323-01-ablation-study.md"
```

## References

@SKILLS/experimentation-protocol.md
@skills/experiment-planning/SKILL.md
@skills/experiment-execution/SKILL.md
@skills/experiment-analysis/SKILL.md
```

- [ ] **Step 2: Commit ablation-study skill**

```bash
git add skills/ablation-study/SKILL.md
git commit -m "feat: add ablation-study skill"
```

---

## Task 10: Specialized Skill - Hyperparameter Sweep

**Files:**
- Create: `skills/hyperparameter-sweep/SKILL.md`

- [ ] **Step 1: Create hyperparameter-sweep skill**

```markdown
# Hyperparameter Sweep

Grid or random search across hyperparameter space with systematic tracking.

## When To Use

**Triggers:**
- "sweep"
- "grid search"
- "tune hyperparameters"
- "find best config"

**Use when:** User wants to systematically search hyperparameter space.

**Don't use when:** Only testing a single config (use regular experiment workflow).

## The Process

### 1. Define Search Space

**Ask user:**
- "Which hyperparameters to tune?"
- "What ranges/values for each?"

Example:
```
learning_rate: [0.001, 0.0001, 0.00001]
batch_size: [32, 64, 128]
dropout: [0.0, 0.1, 0.2, 0.5]
```

### 2. Choose Search Strategy

**Ask user:**
- "Grid search (all combinations) or random search (N random samples)?"

**Grid search:**
- Exhaustive, tests all combinations
- Can be very large (product of all parameter counts)

**Random search:**
- Tests N random combinations
- More efficient for high-dimensional spaces
- Ask: "How many random samples?"

### 3. Generate Experiment Plans

For each config in the sweep:
1. Generate experiment ID: `EXP-YYYYMMDD-XX-sweep-NN`
2. Create experiment plan using `experiment-planning` skill
3. Hypothesis: "Config [params] will improve over baseline"
4. Use same seed, data, commit across all sweep experiments

### 4. Update LEDGER

Add sweep tracking to LEDGER:

```markdown
## Sweep: EXP-YYYYMMDD-XX-sweep
- **Status:** In Progress
- **Total experiments:** 12
- **Completed:** 0/12
- **Best so far:** -
```

### 5. Execute Sweep

For each experiment in the sweep:
1. Run using `experiment-execution` skill
2. Analyze using `experiment-analysis` skill
3. Update LEDGER with progress
4. Track best config so far

**Optionally run in parallel** (if resources allow):
- "I can run up to N experiments in parallel. How many?"

### 6. Generate Sweep Report

After all experiments complete:

```markdown
# Hyperparameter Sweep: EXP-YYYYMMDD-XX-sweep

## Search Space
- learning_rate: [0.001, 0.0001, 0.00001]
- batch_size: [32, 64, 128]
- dropout: [0.0, 0.1, 0.2, 0.5]

## Strategy
- Grid search (36 combinations)

## Results

| Config | Accuracy | Rank |
|--------|----------|------|
| lr=0.0001, bs=64, do=0.1 | 86.2% | 🥇 |
| lr=0.0001, bs=128, do=0.1 | 85.9% | 🥈 |
| lr=0.001, bs=64, do=0.1 | 85.7% | 🥉 |
| ... | ... | ... |

## Best Config
- **learning_rate:** 0.0001
- **batch_size:** 64
- **dropout:** 0.1
- **Accuracy:** 86.2% (+0.7% vs baseline)

## Recommendations
- Use best config for future experiments
- learning_rate=0.0001 consistently performs well
- Larger batch sizes (128) don't help
```

Save to: `experiments/EXP-YYYYMMDD-XX-sweep-report.md`

### 7. Update Base Experiment

Link to sweep report from base experiment plan.

## Common Mistakes

**❌ Too large search space:**
- Warn user: "This will generate 100 experiments. Are you sure?"

**❌ Different seeds across sweep:**
- Use same seed for fair comparison

**❌ Not tracking sweep in LEDGER:**
- Update LEDGER with sweep progress

## Example Dialogue

```
User: "Do a hyperparameter sweep"

Agent: "I'll help you design a sweep.

Which hyperparameters to tune?
User: "learning rate, batch size, dropout"

Agent: "What ranges?
User: "lr: 0.001 to 0.00001, bs: 32-128, dropout: 0-0.5"

Agent: "Grid search or random search?
User: "grid search"

Agent: "This will generate 36 experiments. Proceed?
User: "yes"

Agent: "Creating 36 experiment plans...
[Creates all experiments]

Sweep: EXP-20260323-02-sweep
- Total: 36 experiments
- Status: In Progress

Running experiment 1/36: lr=0.001, bs=32, do=0.0...
[Runs and analyzes...]

Running experiment 2/36: lr=0.001, bs=32, do=0.1...
...

Sweep complete!

Best config: lr=0.0001, bs=64, do=0.1 → 86.2%

Report saved to: experiments/EXP-20260323-02-sweep-report.md"
```

## References

@SKILLS/experimentation-protocol.md
@skills/experiment-planning/SKILL.md
@skills/experiment-execution/SKILL.md
@skills/experiment-analysis/SKILL.md
```

- [ ] **Step 2: Commit hyperparameter-sweep skill**

```bash
git add skills/hyperparameter-sweep/SKILL.md
git commit -m "feat: add hyperparameter-sweep skill"
```

---

## Task 11: Specialized Skill - Baseline Comparison

**Files:**
- Create: `skills/baseline-comparison/SKILL.md`

- [ ] **Step 1: Create baseline-comparison skill**

```markdown
# Baseline Comparison

Head-to-head comparison between two models/configs to determine which is better.

## When To Use

**Triggers:**
- "compare models"
- "beat baseline"
- "model A vs model B"
- "which is better"

**Use when:** User has two experiments to compare directly.

**Don't use when:** Only one experiment exists (need two for comparison).

## The Process

### 1. Identify Experiments to Compare

**Ask user:**
- "Which experiment IDs to compare?"

Or suggest:
- "I see you just ran EXP-20260323-01. Compare vs baseline EXP-20260320-05?"

### 2. Load Both Experiments

Load:
- Experiment A: `experiments/EXP-YYYYMMDD-AA.md`
- Experiment B: `experiments/EXP-YYYYMMDD-BB.md`

### 3. Extract Metrics

From Results section of both:
- Primary metrics
- Secondary metrics
- Runtime, cost
- Resource usage

### 4. Run Statistical Significance (if applicable)

If metrics have variance/stddev:
- Ask user: "Do you have multiple runs with stddev?"
- If yes: Run t-test or similar
- Report: p-value, confidence interval

### 5. Generate Comparison Table

```markdown
# Model Comparison: EXP-AA vs EXP-BB

## Primary Metrics

| Metric | EXP-AA | EXP-BB | Delta | Winner |
|--------|--------|--------|-------|--------|
| Accuracy | 85.5% | 84.2% | +1.3% | EXP-AA |
| F1 Score | 0.83 | 0.84 | +0.01 | EXP-BB |

## Secondary Metrics

| Metric | EXP-AA | EXP-BB | Delta | Winner |
|--------|--------|--------|-------|--------|
| Training time | 1.5h | 1.2h | -0.3h | EXP-BB |
| GPU memory | 8GB | 6GB | -2GB | EXP-BB |
| Inference latency | 50ms | 35ms | -15ms | EXP-BB |

## Statistical Significance
- p-value: 0.03 (significant at α=0.05)
- Confidence interval: [0.5%, 2.1%]

## Overall Winner: EXP-AA

**Reasoning:**
- EXP-AA has significantly better accuracy (+1.3%)
- EXP-BB is faster and cheaper
- If accuracy is primary goal → EXP-AA
- If cost/latency matters → EXP-BB
```

### 6. Declare Winner and Recommend

**Overall winner:** The experiment that wins on primary metric(s)

**Recommendation:**
- "Use EXP-AA as the new baseline for accuracy-critical work"
- "Use EXP-BB for cost-sensitive applications"

### 7. Update Experiments

Link comparison from both experiment plans:

```markdown
## Comparisons
- Vs EXP-BB: [Comparison Report](./EXP-AA-vs-EXP-BB-comparison.md)
```

## Common Mistakes

**❌ Ignoring secondary metrics:**
- Primary metric wins but cost explodes → mention tradeoff

**❌ No clear winner:**
- "Results mixed. EXP-AA better on accuracy, EXP-BB better on speed. Depends on priority."

**❌ Small differences without significance:**
- "Difference 0.1% ± 0.5% - not statistically significant"

## Example Dialogue

```
User: "Compare EXP-20260323-01 vs baseline"

Agent: "Comparing EXP-20260323-01 vs EXP-20260320-05...

Primary Metrics:
- Accuracy: 85.5% vs 82.3% (+3.2%) → EXP-20260323-01
- F1: 0.83 vs 0.82 (+0.01) → EXP-20260323-01

Secondary Metrics:
- Training time: 1.5h vs 1.2h (+0.3h) → EXP-20260320-05
- GPU memory: 8GB vs 6GB (+2GB) → EXP-20260320-05

Overall Winner: EXP-20260323-01
- Significant accuracy gain (+3.2%)
- Worth extra compute cost

Recommendation: Use EXP-20260323-01 as new baseline for accuracy-critical work.

Report saved to: experiments/EXP-20260323-01-vs-EXP-20260320-05-comparison.md"
```

## References

@SKILLS/experimentation-protocol.md
@skills/experiment-analysis/SKILL.md
```

- [ ] **Step 2: Commit baseline-comparison skill**

```bash
git add skills/baseline-comparison/SKILL.md
git commit -m "feat: add baseline-comparison skill"
```

---

## Task 12: Quality Skill - Reproducibility Check

**Files:**
- Create: `skills/reproducibility-check/SKILL.md`

- [ ] **Step 1: Create reproducibility-check skill**

```markdown
# Reproducibility Check

Verify that an experiment can be recreated exactly from its documentation.

## When To Use

**Triggers:**
- "reproduce"
- "can you recreate"
- "verify reproducibility"
- "is this reproducible"

**Use when:** User wants to verify an experiment can be recreated.

**Don't use when:** Experiment plan doesn't exist yet.

## The Process

### 1. Load Experiment Plan

Read: `experiments/EXP-YYYYMMDD-XX.md`

### 2. Verify Reproducibility Metadata

Check all required fields are present:

| Field | Required | Location |
|-------|----------|----------|
| Config path | ✓ | Reproducibility section |
| Exact command | ✓ | Reproducibility section |
| Seed(s) | ✓ | Reproducibility section |
| Data version | ✓ | Dataset section |
| Commit hash | ✓ | Reproducibility section |

**If missing:**
- "Cannot reproduce - [field] is missing. Please add to experiment plan."

### 3. Verify Artifacts Exist

Check:
- [ ] Config file exists at specified path
- [ ] Dataset version is available
- [ ] Git commit is reachable

**If missing:**
- "Config file not found: [path]. Cannot reproduce."

### 4. Check Git Commit

Compare plan's commit hash vs current HEAD:

```bash
git rev-parse HEAD
```

**If different:**
- "Commit mismatch. Plan: abc123, Current: def456. Checkout abc123?"
- If user agrees: `git checkout abc123`

### 5. Attempt Recreate

**Run exact command from plan:**

```bash
# From plan's "Exact command" field
<exact command from plan>
```

### 6. Compare Outputs

Compare new run vs original:
- Metrics should match (or very close)
- Runtime should be similar
- No new errors/warnings

### 7. Generate Reproducibility Report

```markdown
# Reproducibility Check: EXP-YYYYMMDD-XX

## Status: PASS | FAIL

## Metadata Check
- [x] Config path present
- [x] Exact command present
- [x] Seed specified
- [x] Data version specified
- [x] Commit hash recorded

## Artifact Check
- [x] Config file exists: experiments/configs/EXP-XXXX-config.yaml
- [x] Dataset available: data-v3
- [x] Commit reachable: abc123

## Recreate Attempt
- Original metric: 85.5%
- Recreated metric: 85.4%
- Delta: -0.1% (within acceptable variance)

## Conclusion
✅ Experiment is reproducible
- Metrics match within acceptable variance
- All artifacts available
- Command executes correctly
```

Save to: `experiments/EXP-YYYYMMDD-XX-reproducibility.md`

### 8. Update Experiment Plan

Link reproducibility report:

```markdown
## Reproducibility
- Status: Verified PASS
- Report: [EXP-XXXX-reproducibility.md](./EXP-XXXX-reproducibility.md)
```

## Common Mistakes

**❌ Missing metadata:**
- "Cannot reproduce - seed not specified. Add to plan first."

**❌ Wrong commit:**
- Warn user and offer to checkout correct commit

**❌ Config file missing:**
- "Config file not found. Restore from backup?"

## Example Dialogue

```
User: "Verify EXP-20260323-01 is reproducible"

Agent: "Checking reproducibility of EXP-20260323-01...

Metadata check:
- Config path: ✓
- Exact command: ✓
- Seed: ✓ (42)
- Data version: ✓ (v3)
- Commit hash: ✓ (abc123)

Artifact check:
- Config file: ✓
- Dataset: ✓
- Commit reachable: ✓

Commit mismatch detected:
- Plan: abc123
- Current HEAD: def456

Checkout abc123 to reproduce? [y/n]
User: "y"

[Checks out abc123, runs command...]

Original: 85.5%
Recreated: 85.4%
Delta: -0.1%

✅ Experiment is reproducible (within acceptable variance)

Report saved to: experiments/EXP-20260323-01-reproducibility.md"
```

## References

@SKILLS/experimentation-protocol.md
```

- [ ] **Step 2: Commit reproducibility-check skill**

```bash
git add skills/reproducibility-check/SKILL.md
git commit -m "feat: add reproducibility-check skill"
```

---

## Task 13: Quality Skill - Verification Before Completion

**Files:**
- Create: `skills/verification-before-completion/SKILL.md`

- [ ] **Step 1: Create verification-before-completion skill**

```markdown
# Verification Before Completion

Evidence before claims - prevents false success assertions by verifying actual results.

## When To Use

**Triggers:**
- Auto-invoked before any completion claim
- Before marking experiment as WIN
- Before saying "this works"
- User: "It's done!" or "It works!"

**Use when:** Agent or user is about to claim success/completion.

**Don't use when:** Still in the middle of work (no claim being made).

## The Process

### 1. Detect Completion Claim

Agent or user says:
- "It works!"
- "Done!"
- "Success!"
- "Fixed it!"
- "Experiment improved by X%"

### 2. Identify What Was Claimed

Extract the claim:
- "Fixed the bug" → What bug? Where?
- "Improved accuracy" → From what to what?
- "It works" → What does "works" mean?

### 3. Determine Verification Method

**For code fixes:**
- Run the failing test that was failing
- Reproduce the original bug scenario
- Check error is gone

**For experiments:**
- Check actual metrics file (not just printed output)
- Verify against acceptance criteria
- Confirm artifacts exist

**For features:**
- Test the feature manually or with tests
- Verify expected behavior

### 4. Run Verification

**Execute verification command:**

Example for experiments:
```bash
# Check actual metrics from file
cat experiments/artifacts/EXP-XXXX/metrics.json | jq '.accuracy'

# Compare vs acceptance criteria
# (automated check)
```

Example for bugs:
```bash
# Run the test that was failing
pytest tests/test_bug.py::test_specific_bug -v
```

### 5. Compare Claim vs Reality

| Claim | Reality | Action |
|-------|---------|--------|
| "Improved to 85%" | Actual: 82.3% | **BLOCK** - False claim |
| "Bug fixed" | Test still fails | **BLOCK** - Not actually fixed |
| "It works" | Error on run | **BLOCK** - Doesn't work |
| "85.5% accuracy" | Actual: 85.5% | **ALLOW** - Claim verified |

### 6. Take Action

**If claim matches reality:**
- Allow completion
- "Claim verified. Proceeding."

**If claim doesn't match reality:**
- **BLOCK the completion**
- "Claim doesn't match reality. You said X, but actual is Y."
- "Please re-run / re-check before claiming completion."

**If verification fails:**
- "Verification failed. Cannot claim completion."
- Suggest next action: re-run, fix, investigate

## Common Mistakes

**❌ Trusting printed output:**
- User: "It says 85%!"
- Check: Actual file says 82%
- Block: "Printed output doesn't match actual results"

**❌ Not verifying:**
- "You claim it works but haven't tested. Run verification first."

**❌ Blocking without reason:**
- Always explain what was claimed vs what reality is

## Example Dialogue

```
User: "Experiment done! Accuracy improved to 87%!"

Agent: "Verifying claim...
Claim: Accuracy improved to 87%
Checking actual metrics...

[Reads experiments/artifacts/EXP-XXXX/metrics.json]
Actual accuracy: 85.5%

❌ Claim doesn't match reality
- Claimed: 87%
- Actual: 85.5%
- Delta: +1.5% (false claim)

Please verify actual results before claiming completion.
The actual improvement is 82.3% → 85.5% (+3.2%), which still meets acceptance criteria."

---

User: "Bug is fixed!"

Agent: "Verifying fix...
Running test that was failing...

[Runs pytest tests/test_bug.py::test_specific_bug]
PASS

✅ Claim verified - bug is actually fixed
Proceeding with completion."
```

## Iron Rule

**NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE**

Evidence first, assertions second. Always.

## References

@SKILLS/experimentation-protocol.md
```

- [ ] **Step 2: Commit verification-before-completion skill**

```bash
git add skills/verification-before-completion/SKILL.md
git commit -m "feat: add verification-before-completion skill"
```

---

## Task 14: Documentation and Final Polish

**Files:**
- Modify: `README.md` (add installation examples)
- Create: `docs/INSTALL.md` (detailed installation guide)
- Create: `docs/WORKFLOW.md` (workflow examples)

- [ ] **Step 1: Update README with installation examples**

Add to README.md after Installation section:

```markdown
## Quick Start

1. Install the plugin:
```bash
/plugin install /path/to/superpowers-lab
```

2. Start a new session and say:
```text
I want to test if adding attention helps my model
```

3. Superpowers Lab will guide you through:
- Creating an experiment plan
- Running with reproducibility tracking
- Analyzing results vs baseline
- Deciding next steps

## Example Workflow

See [docs/WORKFLOW.md](docs/WORKFLOW.md) for detailed examples.
```

- [ ] **Step 2: Create installation guide**

Create `docs/INSTALL.md`:

```markdown
# Installation Guide

## Claude Code (Local Path)

```bash
# From local directory
/plugin install /home/user/superpowers-lab
```

## Verification

Start a new session and ask:

```text
Help me design an experiment
```

The agent should invoke the `experiment-planning` skill automatically.

## Manual Installation

If the plugin system doesn't work:

1. Copy skills to your Claude skills directory
2. Add session-start hook to your hooks
3. Restart Claude Code

See Claude Code documentation for manual installation details.

## Troubleshooting

**Skills not triggering:**
- Ensure session-start hook is running
- Check skills are in correct directory
- Try: "Use experiment-planning skill" (manual invocation)

**File permission errors:**
- Ensure hooks/session-start is executable: `chmod +x hooks/session-start`
```

- [ ] **Step 3: Create workflow examples**

Create `docs/WORKFLOW.md`:

```markdown
# Workflow Examples

## Example 1: First Experiment

**Goal:** Test if a transformer layer improves sequence modeling

```text
You: I want to test if adding a transformer layer helps my LSTM model

Agent: I'll help you design an experiment. Let me ask some questions...

[Questionnaire about hypothesis, baseline, metrics, etc.]

Agent: Experiment plan created: EXP-20260323-01
- Hypothesis: Transformer improves accuracy
- Baseline: 82.3% (current best)
- Target: ≥84.0%
- Risk: 20-30% slower training
- Seed: 42, data v3

Say "run the experiment" to begin.

You: run the experiment

Agent: Running EXP-20260323-01...
[Monitors training...]

Agent: Analysis complete:
- Accuracy: 82.3% → 85.5% (+3.2%)
- Outcome: WIN, Confidence: High
- Artifacts saved to: experiments/artifacts/EXP-20260323-01/

Should we run an ablation study?
```

## Example 2: Ablation Study

```text
You: Run an ablation study on EXP-20260323-01

Agent: I'll ablate the components...
- Remove transformer: 82.1% (-3.4%)
- Remove layer norm: 84.8% (-0.7%)
- Remove dropout: 85.8% (+0.3%)

Recommendation: Keep transformer, keep layer norm, remove dropout.
```

## Example 3: Hyperparameter Sweep

```text
You: Do a hyperparameter sweep on learning rate and batch size

Agent: Creating sweep...
- lr: [0.001, 0.0001, 0.00001]
- batch_size: [32, 64, 128]
- Total: 9 experiments

Running 1/9...
[...]
Sweep complete!

Best config: lr=0.0001, bs=64 → 86.2%
```
```

- [ ] **Step 4: Commit documentation**

```bash
git add README.md docs/INSTALL.md docs/WORKFLOW.md
git commit -m "docs: add installation guide and workflow examples"
```

---

## Task 15: Testing and Validation

**Files:**
- Create: `tests/` (structure for future tests)
- Create: `tests/README.md` (testing guide)

- [ ] **Step 1: Create testing structure**

```bash
mkdir -p tests
```

- [ ] **Step 2: Create testing guide**

Create `tests/README.md`:

```markdown
# Testing Guide

## Manual Testing Checklist

### Session-Start Hook
- [ ] Hook runs at session start
- [ ] Protocol is injected into context
- [ ] No errors in hook execution

### Experiment Planning
- [ ] Triggers on "run an experiment"
- [ ] Asks all required questions
- [ ] Creates experiment plan document
- [ ] Updates LEDGER
- [ ] Generates valid experiment ID

### Experiment Execution
- [ ] Loads existing experiment plan
- [ ] Runs the specified command
- [ ] Captures output
- [ ] Updates experiment status
- [ ] Saves artifacts

### Experiment Analysis
- [ ] Loads experiment and baseline
- [ ] Calculates deltas correctly
- [ ] Determines correct outcome label
- [ ] Updates LEDGER
- [ ] Suggests next action

### Ablation Study
- [ ] Identifies components to ablate
- [ ] Creates child experiments
- [ ] Uses same seed/data across all
- [ ] Generates comparison report

### Hyperparameter Sweep
- [ ] Generates all combinations (grid) or samples (random)
- [ ] Tracks progress in LEDGER
- [ ] Identifies best config
- [ ] Generates sweep report

### Baseline Comparison
- [ ] Loads both experiments
- [ ] Generates comparison table
- [ ] Declares winner correctly
- [ ] Handles mixed results

### Reproducibility Check
- [ ] Verifies all metadata present
- [ ] Checks artifacts exist
- [ ] Attempts recreation
- [ ] Reports PASS/FAIL correctly

### Verification Before Completion
- [ ] Detects completion claims
- [ ] Runs verification command
- [ ] Blocks false claims
- [ ] Allows verified claims

## Test Projects

Create test projects to validate end-to-end workflows:

### Test 1: Simple Classification
- Binary classification task
- Compare logistic regression vs random forest
- Verify experiment planning and analysis

### Test 2: Hyperparameter Sweep
- Small grid search (2x2x2 = 8 experiments)
- Verify sweep tracking and reporting

### Test 3: Ablation Study
- Model with 3 components
- Verify ablation matrix and report

## Integration Testing

Test with Claude Code:
```bash
# Install plugin locally
/plugin install /path/to/superpowers-lab

# Start new session
# Test: "Help me design an experiment"
```
```

- [ ] **Step 3: Commit testing structure**

```bash
git add tests/README.md
git commit -m "test: add testing guide and checklist"
```

---

## Task 16: Final Review and Tag

**Files:**
- None (repo-level tasks)

- [ ] **Step 1: Review all skills for consistency**

Check:
- [ ] All skills reference @SKILLS/experimentation-protocol.md
- [ ] All skills follow same structure (When To Use, The Process, Common Mistakes, Examples)
- [ ] No TODOs or placeholders remain
- [ ] All file paths are correct

- [ ] **Step 2: Verify plugin manifest**

Check:
- [ ] All 8 skills listed
- [ ] SessionStart hook points to correct file
- [ ] Version is 1.0.0

- [ ] **Step 3: Create initial release**

```bash
# Tag release
git tag -a v1.0.0 -m "Initial release of Superpowers Lab"

# Push to remote (when ready)
# git remote add origin <url>
# git push -u origin main
# git push origin v1.0.0
```

- [ ] **Step 4: Create CHANGELOG**

Create `CHANGELOG.md`:

```markdown
# Changelog

## [1.0.0] - 2026-03-23

### Added
- Initial release of Superpowers Lab
- 8 core skills for disciplined ML experimentation
- Session-start hook for protocol injection
- Experiment plan template
- Comprehensive documentation

### Skills
- experiment-planning - Structured questionnaire → written plan
- experiment-execution - Run with reproducibility tracking
- experiment-analysis - Compare vs baseline, collect artifacts
- ablation-study - Systematic component removal
- hyperparameter-sweep - Grid/random search tracking
- baseline-comparison - Head-to-head model comparison
- reproducibility-check - Verify experiment can be recreated
- verification-before-completion - Evidence before claims

### Documentation
- Installation guide
- Workflow examples
- Testing guide
- Experimentation protocol reference
```

- [ ] **Step 5: Final commit**

```bash
git add CHANGELOG.md
git commit -m "chore: add changelog for v1.0.0"
```

---

## Summary

**Total tasks:** 16
**Estimated time:** 4-6 hours for implementation

**Key deliverables:**
- ✅ 8 skills covering full experimentation lifecycle
- ✅ Session-start hook for protocol injection
- ✅ Experiment plan template
- ✅ Comprehensive documentation
- ✅ Testing framework

**Next steps after implementation:**
1. Manual testing with real ML experiments
2. Gather user feedback
3. Iterate on skill wording
4. Consider integration with MLflow/W&B
5. Add support for other platforms (Cursor, Codex, etc.)
