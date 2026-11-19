# Council Review

An open-source AI skill that runs a manual "LLM council" across any models you have access to.

Pose a hard question to several AI models, have a reviewer rank their anonymous answers, then have a chairman merge the best parts into one final answer. The process works in any chat interface, with any models, and needs no code or API keys.

## How it works

1. **First opinions:** send the same question to 3 to 4 models in separate chats.
2. **Anonymize:** the assistant removes model identifiers and shuffles the answers.
3. **Rank:** a reviewer model ranks the anonymous answers with reasons.
4. **Synthesize:** a chairman model writes one final answer from the answers and the ranking.
5. **Reveal:** the model key is shown only at the end.

## Using it

**With a skill-capable assistant (e.g., Claude):** install the `council-review` folder as a custom skill, then ask, "Run a council review on this question: ..." or paste several answers and ask it to run the council.

**With any other assistant:** paste the contents of `SKILL.md` into the chat as instructions, then follow the same steps. The templates in `templates/` give you copy-ready prompts for each step.

## Repository layout

```
council-review/
├── SKILL.md              # Skill instructions (the core file)
├── README.md
├── LICENSE
└── templates/
    ├── first-opinion.md  # Step 1 prompt
    ├── ranking.md        # Step 3 prompt
    └── chairman.md       # Step 4 prompt
```

## Limitations

- Models are not truly independent when they share training data or are from the same provider.
- Anonymization removes obvious identifiers, but writing style can still hint at the author.
- Reviewer and chairman choices affect the output. Use different models for each role where possible.
- Costs scale with the number of models. For simple questions, one model is enough.

## Inspiration

Based on the idea behind [karpathy/llm-council](https://github.com/karpathy/llm-council).

## License

MIT. See [LICENSE](LICENSE).
