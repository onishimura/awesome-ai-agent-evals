# Contributing

Thanks for helping keep this list useful. It is curated, so quality matters more than coverage.

## Inclusion criteria

An entry must meet all of the following:

1. **Agent-focused.** It evaluates AI agents: multi-step systems that use tools, browse, write and run code, or operate computers. General LLM evals qualify only if they're a staple that agent builders actually use, such as foundational LLM-as-judge work.
2. **Maintained or citable.** For a repo, that means recent activity and not archived. For a paper, it means published or widely cited. Superseded benchmarks belong under their current successor; mention the predecessor only if it's still widely cited.
3. **Primary source.** Link the arXiv paper, official repository, or official docs/site. Don't link news articles, blog rehashes, aggregator pages or mirrors.
4. **No paid or affiliate promotion.** Commercial tools are fine only if they're open source or have a usable free tier with real eval features (this applies to Tracing & observability). Vendor-self-reported leaderboards are not accepted. Leaderboards must be independent or run by the benchmark authors.
5. **Right section.** A benchmark paper goes with its benchmark entry; *Key papers* is for surveys and methodology only.

## Entry format

```markdown
- [Name](primary-source-url) - One line: what it measures / why it matters.
```

- Keep entries alphabetized within their section, ignoring case. Sort `τ` as `tau`.
- Write one sentence, or two short ones. Note version caveats briefly, for example "scores not comparable with v1".
- Don't add marketing language, emojis or star counts.

## Pull requests

- One entry per PR is preferred. In the PR description, say why it made the cut and give its last activity date.
- The link checker (lychee) runs on every PR and must pass.
- If you're removing or replacing a stale entry, link the evidence (archived repo, deprecation notice, successor).
