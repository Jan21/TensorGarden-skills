---
name: ml-paper-writing
description: Use when drafting, restructuring, or critiquing a machine learning research paper (abstract, introduction, related work, method, experiments, figures, citations, notation, style), or when the user asks to write/edit in a specific ML-paper style, propose a section outline, or review draft text for clarity, overclaiming, passive voice, vague language, missing baselines, weak captions, or sloppy citations.
---

# ML Paper Writing

Reference distilled from Jakob Foerster's *How to ML Paper*, Zachary Lipton's *Heuristics for Scientific Writing*, and Grigorios Chrysos's *How to Write a Research Paper in Machine Learning*.

## Modes of Use

When the user invokes this skill, pick the mode from their request:

- **Draft**: write a section (abstract, intro, related work, etc.) from notes or bullet points.
- **Structure**: propose or critique a section outline given a topic, contribution, and target venue.
- **Critique style**: review prose for passive voice, intensifiers, vague adjectives, overclaiming, anthropomorphizing algorithms, filler, long sentences.
- **Critique content**: review claims, baselines, ablations, comparisons, citation choices, notation consistency.
- **Polish**: line-edit existing text preserving meaning, applying the style rules below.

If the mode is ambiguous, ask which one before proceeding. For critique modes, output findings as `location: issue. fix.` lines, not paragraphs of praise.

## Canonical Paper Structure

| Section | Purpose | Key moves |
|---|---|---|
| Title | One-line pitch | Concrete, no jargon-soup |
| Abstract | 2-minute spotlight | X-Y-Z-1 formula (below) |
| Introduction | Expanded abstract | Lead with example, end with bulleted contributions |
| Related Work | Compare & contrast | Differentiate, do not just list |
| Background / Problem Setting | Formalism, notation | Define everything used later |
| Method | What you do, why | Reuse formalism from Background |
| Experimental Setup | How you test | Datasets, baselines, hyperparameters, compute |
| Results & Discussion | What you found | Tables, ablations, statistical measures, limitations |
| Conclusion | Recap + future work | Short. No new claims. |

Write in the order **you find easiest**, not the order sections appear. Drafting the abstract first (even before final results) sharpens focus.

## Abstract

**X-Y-Z-1 formula** (Foerster):
1. **X**: what you attempt and why it matters.
2. **Y**: why it is hard, or where prior work falls short.
3. **Z**: your solution in one sentence.
4. **1**: how you verify it (experiments, theory) plus the headline number.

Rules:
- Keep short. No teasing; put the headline result in.
- Concrete numbers belong here ("+4.2 BLEU on WMT14", not "significantly improves").
- No generic opener ("The last 10 years have witnessed...").
- Do not include citations or undefined acronyms.

## Introduction

Expanded abstract. Structure:

1. Concrete motivating example or problem instance.
2. Formalize it (one sentence).
3. Why existing approaches fail or are incomplete (compare, do not just list).
4. Your method in one paragraph.
5. **Bulleted contribution list**: 3 to 5 bullets, each starting with a verb ("We propose…", "We prove…", "We show empirically that…").

Arrive at the contribution within the first page. Cut every sentence that could appear in any other ML paper.

Focus on what your method **does**, not what it doesn't.

## Related Work

**Compare and contrast.** Group prior work by approach, then explain how yours differs in assumption, scope, or mechanism. A list of citations with one-sentence summaries each is not related work.

- Cite generously. You have nothing to lose by including a relevant paper; you have reviewers to lose by omitting one.
- Cite throughout the paper, not only in this section. Whenever you invoke a method, dataset, or claim that isn't yours, cite it on the spot.
- If page limits allow, fill the bibliography. Empty space signals shallow reading.
- Cite the **published** version (conference / journal identifier), not the arXiv preprint, when both exist.
- For datasets and benchmarks, cite the original paper introducing them.

## Method

- Use the formalism defined in Background. Do not re-introduce notation.
- State each design choice and **why**. A method section without justifications is a recipe, not research.
- Algorithms blocks help. Pseudo-code that matches the equations builds trust.

## Experiments

Seven-step recipe (Chrysos):

1. Drop the table or main figure in first, even with placeholder numbers.
2. Write 2-3 sentences explaining the comparison.
3. State **why the reader should care** about this experiment.
4. Describe datasets (with citations).
5. Implementation details: hyperparameters, hardware, seeds, runtime.
6. Add ablations / additional experiments to isolate the contribution.
7. Re-order for logical flow; the paper is not a diary.

Must-haves:
- Baselines that are actually comparable (same data, same compute budget where possible).
- Error bars, std dev, or confidence intervals over multiple seeds.
- An ablation per claimed contribution.
- A **limitations** paragraph. Honest limitations build credibility; missing ones get caught by reviewers.

## Figures and Tables

- **Standalone-readable.** Captions explain what is shown, what to look at, and the conclusion. "Accuracy on CIFAR10" is not a caption.
- Captions 1-3 lines, ending with the takeaway.
- Indicate direction with arrows: "Accuracy (↑)", "Latency in ms (↓)".
- **Bold the best value** per metric per experiment.
- Consistent styling across the paper (same colors per method, same axis ranges where comparable).
- Reviewers skim figures first. Your figures must tell the story alone.

## Notation and Math

- All symbols in math mode, in text too: write `$K$`, not "K".
- Pick a convention (e.g., bold lowercase for vectors, bold uppercase for matrices) and **never break it**.
- Avoid overloading: do not use both `$u$` and `$\upsilon$`; do not reuse the same symbol for two things.
- Equations end with punctuation: comma if the sentence continues, period if it ends.
- Label every equation, section, table, figure (`\label{eq:loss}`, `\label{sec:method}`, `\label{fig:arch}`). No duplicates.
- Acronyms: define on first use, use the abbreviation thereafter. Use `\newcommand{\cifarten}{CIFAR-10}` to enforce a single spelling.

## Citations (LaTeX)

- `\citet{foo2024}` when the authors are the grammatical subject, e.g., "Foerster et al. (2024) show that…".
- `\citep{foo2024}` otherwise, e.g., "…has been studied (Foerster et al., 2024)".
- Never write "The authors in \citet{foo2024} show…"; write "\citet{foo2024} show…".
- Use `\usepackage[backref=page]{hyperref}` so reviewers can jump back to where each citation is used.

## Style Rules (sentence level)

Bad/why/fix table. Each row is a specific rewrite trigger to scan for.

| Bad | Why | Fix |
|---|---|---|
| "The model is trained on ImageNet." | Passive obscures agency (Foerster's most-flagged mistake). | "We train the model on ImageNet." |
| "Our method is very effective." | Intensifier (very, extremely, incredibly, truly, really) signals insecurity, not strength (Lipton). | Drop, or quantify with a number. |
| "Rich representations." | Vague descriptor (rich, complex, promising, powerful, robust, meaningful) commits to nothing (Lipton). | "256-dim representations trained with contrastive loss." |
| "Most prior work uses CNNs." | Overclaim. Unless you cite a survey, "most" is unverifiable. | "Many prior works…" or "Recent work \citep{a,b,c}…". |
| "The algorithm tries to minimize the loss." | Anthropomorphizing. "Algorithms don't try, just as they don't think" (Lipton). | "The algorithm minimizes the loss." |
| "The model knows that X." | Same: AI systems don't "know", "believe", "want", "understand". | "The model predicts X" / "outputs X". |
| "In Section 3 we will show…" | Future tense for paper-internal reference (Foerster). | "Section 3 shows…". |
| "We will train for 100k steps." | Future tense for completed experiments. | "We trained for 100k steps." |
| "In order to achieve X, we…" | Filler; "in order to" → "to" (Foerster). | "To achieve X, we…". |
| "The bank loan problem can be reformulated as…" | Filler; "can be" hedges what is in fact true (Foerster). | "The bank loan problem is…". |
| "It should be noted that the loss is non-convex." | Empty connective; if it should be noted, note it. | "The loss is non-convex." |
| "Note that, as mentioned earlier, …" | Reader knows; cut. | Delete the meta-comment. |
| "We perform an evaluation of the model." | Nominalization; verb buried in noun. | "We evaluate the model." |
| "There is a tendency for the loss to oscillate." | Existential filler hides the subject. | "The loss tends to oscillate." |
| "Our novel state-of-the-art approach revolutionizes…" | Hype words (novel, state-of-the-art, revolutionary, groundbreaking). | Let the numbers say it. |
| "This shows the method is robust." | "This" without antecedent leaves the reader guessing. | "This drop in variance shows the method is robust." |
| "The training, validation, and test sets, respectively, contain…" | "Respectively" with a long list mid-sentence is hard to parse. | Split into two sentences. |
| "We use a 100-layer deep, ResNet-style, residual network." | Stacked adjectives. | "We use a 100-layer ResNet." |
| "The proposed method, which we call FOO, that uses…" | Nested clauses bury the verb. | "We propose FOO, a method that uses…". |
| "etc." in a list of techniques. | Vague; reviewers want specifics. | Name them all, or pick the few that matter. |
| "We achieved very good results on a wide range of tasks." | Triple sin: intensifier, vague, no number. | "We improved BLEU by 4.2 on WMT14 and METEOR by 3.1 on IWSLT." |
| "The paper presents…" | Self-reference in third person. | "We present…". |
| "It is well known that deep networks overfit." | Unsubstantiated; if known, cite. | "Deep networks overfit \citep{zhang2017}." |
| "Recent advances in deep learning have led to…" | Generic opener that fits any ML paper (Foerster). | Lead with the concrete problem. |
| "The contributions of this paper are as follows:" | Boilerplate that wastes a sentence. | Just start the bullet list. |
| "Significantly better" (without a test). | "Significant" has a statistical meaning; do not borrow it casually. | "Better" plus a statistical test, or just the number. |
| "Different from prior work, our method…" | Vague differentiation. | Name the prior work and the specific axis of difference. |
| "Cifar10, CIFAR10, Cifar-10" all in one paper (Chrysos). | Sloppy. | One spelling everywhere; enforce with `\newcommand`. |
| "ResNet [10] was introduced by He et al. [10]." | Double-citing same ref in one sentence. | "ResNet \citep{he2016} introduced…". |
| "The authors in \citet{foo}…" (Chrysos). | `\citet` already names the authors. | "\citet{foo} show…". |
| "Recent work [1,2,3,4,5,6,7,8] has shown…" | Citation dump. | Group by approach; cite each group's representative. |
| ""quotes"" (LaTeX straight). | Renders wrong (Foerster). | ` ``quotes'' ` or `\enquote{quotes}`. |
| "i.e." vs "e.g." swapped. | "i.e." = that is; "e.g." = for example. | Use the right one, comma after both. |
| "Data is" vs "Data are". | Pick a convention. | Most ML papers use "data is" (mass noun); be consistent. |
| "We trained the model on 8 GPUs which took 3 days." | Restrictive vs non-restrictive; missing comma changes meaning. | "…on 8 GPUs, which took 3 days." |

### Other rules (not in the table)

#### Sentence shape
- **Short sentences.** Great scientific writers write mostly in short sentences (Lipton). Split anything over two lines.
- **One idea per sentence; one claim per paragraph topic sentence.** If a sentence has two `and`s connecting independent clauses, split it.
- **Lead with the subject.** Long prepositional preambles ("In the context of large-scale distributed training, we…") bury the verb. Put the subject first.
- **Parallel structure in lists.** Bullets all start with a verb, or all with a noun, never mixed.
- **Avoid stacked modifiers.** "A novel deep contrastive self-supervised approach" hides what is doing the work.
- **Vary sentence length within paragraphs.** Three short sentences in a row read choppy; three long ones read dense.

#### Voice and agency
- **Active voice by default** (Foerster, Lipton). Name who does what. Passive is justified only when the agent is unknown or irrelevant.
- **Scientific "we".** "We" includes the reader: "We see in Figure 2 that…". Avoid "the authors" or "the paper" for self-reference.
- **No second person.** Avoid "you" in a paper.
- **No anthropomorphism.** Models don't "think", "know", "want", "try", "decide", "learn to care about". They minimize losses, output distributions, condition on inputs.
- **Subject-verb-modifier agreement** (Lipton). The grammatical subject must be the thing doing the verb. "Training the model, the accuracy improved" is wrong; the accuracy didn't train.

#### Hedging and claims
- **No hostages to fortune** (Foerster, Lipton). If you are not 100% sure, soften ("suggests", "is consistent with", "we hypothesize") or drop the claim. **A sin of omission beats a sin of commission.**
- **"Many" beats "most" when unsure** (Lipton).
- **Mark opinions explicitly** (Lipton). "In our view…", "We believe…", "We hypothesize…". Stating opinion as fact is the most common style flaw in ML drafts.
- **Avoid weasel quantifiers.** "Some", "often", "frequently", "a number of" without numbers. Replace with a number, a citation, or delete.
- **No unsubstantiated "well known".** If it is well known, cite it.
- **No "significant" without a test.** Reserve the word for statistical significance.

#### Word economy
- **Delete 1/3 of the words** on the first revision pass (Foerster). Most drafts are bloated.
- **Kill filler verbs.** "can be", "shall", "would", "may" when the claim is in fact true. "X can be viewed as Y" → "X is Y".
- **Kill filler phrases.** "in order to", "due to the fact that", "with respect to", "it is worth mentioning that", "as can be seen", "in this paper we", "the goal of this work is to".
- **Prefer verbs to nominalizations.** "We perform an evaluation" → "We evaluate". "Make a comparison" → "compare". "Provide an explanation" → "explain".
- **Cut redundant pairs.** "each and every", "first and foremost", "any and all", "various and sundry".
- **No "etc."** in technical lists. Be exhaustive or pick representatives explicitly.

#### Tense
- **Present tense for what the paper does.** "Section 3 introduces…", "Figure 2 shows…", "We propose…".
- **Past tense for completed experiments.** "We trained for 100k steps", "Accuracy reached 92%".
- **No future tense for paper-internal reference** (Foerster). "We will show in Section 3" → "Section 3 shows".
- **Be consistent within a section.** Switching tense mid-paragraph reads sloppy.

#### Concreteness
- **Prefer concrete to abstract nouns.** "Latency dropped by 30ms" beats "performance improved".
- **Numbers in the abstract and intro.** A claim with a number is checkable; a claim without one is rhetoric.
- **Name baselines and datasets specifically.** "Compared to a strong baseline on a standard benchmark" tells the reader nothing.
- **Avoid hype words.** "novel", "state-of-the-art", "groundbreaking", "revolutionary", "powerful", "unprecedented". Let the result do the work.

#### Reader navigation
- **Forward references with section numbers, not promises.** "As discussed in Section 4" beats "As we will see later".
- **Repeat the term, do not vary it.** Synonyms confuse the reader: pick one name for your method and stick to it.
- **Define every acronym on first use.** Then use the acronym, not the expansion, thereafter.
- **First-letter capitalize proper nouns only** (Foerster). "Transformer" (proper noun, the architecture) is capitalized; "transformers" (generic) is not. "Adam optimizer" is capitalized; "the optimizer" is not.

#### Typography and formatting
- **Curly quotes in LaTeX:** ` ``open and close'' `, never `"straight"`.
- **Non-breaking space before citations:** `the result~\citep{foo}` so the citation never lands on a new line alone.
- **`\,` between number and unit:** `100\,ms`, `8\,GPUs`.
- **No single-word last lines on a paragraph** (orphans/widows). Reword to push or pull a word.
- **Fill the page.** White space at the end of a section signals a half-finished draft (Foerster).
- **`\citet` for in-text, `\citep` for parenthetical.** Mixing them up is the most common citation error.
- **Acronyms with `\usepackage[acronym]{glossaries}`** to keep first-use definitions automatic.

#### Internationalization
- **Simple language.** Your reviewers are not native English speakers. Avoid idioms ("at the end of the day", "low-hanging fruit"), phrasal verbs where a single verb works ("come up with" → "design"), and culture-specific references.
- **One spelling system.** US or UK, not both. Most ML venues use US.
- **No contractions** ("don't", "we're", "it's") in formal text.

#### Contribution clarity
- **Never blur prior work and your contribution** (Foerster). If a sentence could be misread as claiming someone else's idea, rewrite it.
- **"We" only for your work.** Use "prior work shows" or `\citet{}` for others; "We show" for yours.
- **State the delta.** Every contribution sentence should make clear what is new: what changes from the closest prior work.

#### Self-debugging
After each draft pass, grep the file for:
`very`, `extremely`, `truly`, `really`, `incredibly`, `quite`, `rich`, `complex`, `promising`, `powerful`, `novel`, `state-of-the-art`, `we will`, `in order to`, `it should be noted`, `it is worth`, `as can be seen`, `most prior`, `is trained`, `is computed`, `is used`, `tries to`, `knows`, `wants`, `learns to care`, `significantly` (unless followed by a test), `etc`, `well known`.
Each hit is a candidate rewrite.

## Content Rules

- **Never blur prior work and your contribution.** If a sentence could be misread as claiming someone else's idea, rewrite it.
- One claim per sentence; one idea per paragraph (3 sentences minimum).
- A paragraph that does not advance the argument gets cut.
- Tense consistency: present tense for what the paper does ("Section 3 introduces…"), past tense for completed experiments ("We trained for 100k steps").
- International audience: simple language, no idioms.

## Authorship Convention

| Position | Typical role |
|---|---|
| First author | Led the work and writing |
| Second author (`*` if equal) | Substantial second contributor |
| Middle authors | Specific experiments, early input, feedback |
| Second senior author | Day-to-day advising |
| Last/senior author | Project lead, ultimate responsibility |

Err on the side of generous inclusion when uncertain.

## Logistics

- Finish a complete draft **one week** before the deadline. Final week is for polish, plots, and co-author review.
- Use Overleaf with change tracking; share by email so anyone can edit.
- Compile the final PDF and search it for `??` (broken refs) and overfull boxes.

## Red Flags Checklist (use when reviewing a draft)

Run through these in order. Each `yes` is a rewrite item.

- [ ] Abstract has no headline number.
- [ ] Introduction's first sentence could open any ML paper.
- [ ] Contribution bullets missing or buried past page 2.
- [ ] Related Work is a list, not a comparison.
- [ ] A method design choice has no justification.
- [ ] No ablation for a claimed contribution.
- [ ] No error bars / seeds reported.
- [ ] No Limitations paragraph.
- [ ] Figure caption is one short phrase.
- [ ] Tables missing bold best, missing ↑/↓ arrows.
- [ ] Notation reused or inconsistent (e.g., CIFAR10 and Cifar-10 both appear).
- [ ] `\citet` / `\citep` misused.
- [ ] Dataset cited only on first mention then forgotten.
- [ ] Passive voice in main claims.
- [ ] Intensifiers ("very", "extremely", "truly", "really").
- [ ] Vague adjectives ("rich", "complex", "promising") without specifics.
- [ ] Anthropomorphized algorithms ("the model knows / tries / wants").
- [ ] Future tense for paper-internal references ("we will show").
- [ ] Filler phrases ("in order to", "it should be noted that").
- [ ] Sentences over two lines.
- [ ] Prior work and contribution blurred.
- [ ] Citing arXiv when a conference version exists.
- [ ] Broken refs (`??`) in compiled PDF.

## Quick Templates

### Abstract skeleton
> Task `X` is important because `<consequence>`. However, `<approach class>` suffers from `<problem>` (Y). We propose `<method>` which `<key idea>` (Z). On `<dataset>` we achieve `<headline number>`, improving over `<baseline>` by `<delta>` (1).

### Contribution bullets skeleton
> - We propose `<method>`, the first to `<novel property>`.
> - We prove `<formal claim>` under `<assumption>`.
> - Empirically, `<method>` improves `<metric>` by `<delta>` on `<benchmarks>`.
> - We release code and pretrained models at `<url>`.

### Figure caption skeleton
> **Figure N.** `<What is shown>` on `<dataset/setting>`. `<What to look at>`. `<Conclusion the reader should draw>`. Lower is better (↓).

## When NOT to Use

- Non-ML papers (theory-only math, systems, HCI); conventions differ.
- Blog posts, talks, tutorials; these tolerate informality the skill flags as errors.
- Patent applications, theses; different structural requirements.

## Sources

- Foerster, *How to ML Paper: A Brief Guide* (CC BY-NC).
- Lipton, *Heuristics for Scientific Writing (A Machine Learning Perspective)*, Approximately Correct, 2018.
- Chrysos, *How to Write a Research Paper in Machine Learning*.
