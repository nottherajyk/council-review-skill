---
name: council-review
description: Run a multi-model "council" on a hard question. Collect answers from several AI models, anonymize and shuffle them, have a reviewer model rank them, then have a chairman model write a final answer. Use when the user asks for an LLM council, a second opinion from multiple models, to rank several AI answers, or to merge several AI answers into one.
license: MIT
---

# Council Review

A manual, model-agnostic version of the LLM council process. Independent answers are collected, anonymized, ranked by a reviewer, and merged by a chairman. The assistant handles anonymizing, shuffling, prompt-building, and bookkeeping. The user runs the prompts in whichever model chats they choose.

This skill does not require any specific AI platform, tool, or API. It works with any assistant that can read text and write text.

## Modes

**Mode A: the user has a question only.** Go to Step 1.
**Mode B: the user already has several answers.** Go to Step 2.

## Step 1: Collect first opinions (Mode A)

1. Confirm the exact question with the user. Do not reword it.
2. Output one copy-ready block per council model, each containing the full question. Use the template in `templates/first-opinion.md`.
3. Ask the user to paste each model's full answer back, labeled with the model name.
4. Recommended council size: 3 to 4 models. Accept whatever the user has, and note if it is fewer than 3.

## Step 2: Anonymize and shuffle

1. Confirm you have the original question and at least 3 answers. If not, ask for what is missing.
2. Remove identifying text from each answer without changing its substance:
   - Self-references to the model or company (e.g., "As an AI developed by...").
   - Sign-offs, branding, or formatting unique to one model.
   - Keep all content, numbers, and reasoning unchanged.
3. Shuffle the answers into random order. Label them Response 1, 2, 3, and so on in that order.
4. Keep a private key mapping each label to its model name (e.g., `Response 1 = Model C`). Do not reveal the key until Step 5.

## Step 3: Ranking prompt

Give the user the ranking prompt from `templates/ranking.md`, filled in. The reviewer should be a model that did not write the response it is most likely to favor. If the user can't tell, note the risk of bias. Ask the user to paste the reviewer's ranking back. A second reviewer is optional, for comparison.

## Step 4: Chairman prompt

Once the ranking is back, give the user the chairman prompt from `templates/chairman.md`, filled in. The chairman should be a different model from the reviewer where possible. Ask the user to paste the chairman's output back.

## Step 5: Deliver the result

After the chairman's answer is pasted in, reply with:

1. The final answer, as written by the chairman. Do not edit it unless the user asks.
2. The model key, showing which model wrote each response, who ranked, and who chaired.
3. A short note on any disagreement between the ranking and the answers, if relevant.

## Rules

- Do not reveal the model key before the final answer. Blinding prevents bias in the ranking.
- Keep the question and response text exact, except for removing identifying text in Step 2.
- The user is in control. If they want a different reviewer, chairman, or ranking criteria, rebuild the prompt.
- With fewer than 3 answers, say a council needs at least 3 and ask for more, or proceed only with the user's explicit approval.
- For simple factual questions, note that a single answer is usually enough.
