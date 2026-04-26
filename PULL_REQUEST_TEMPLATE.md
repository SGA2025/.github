<!-- Thank you for contributing to an SGA2025 repository. -->
<!-- Please complete this template before requesting review. -->

## What this PR does

<!-- One or two sentences summarizing the change -->

## Risk classification

Choose ONE:

- [ ] 🟢 **Green** — Content, styling, copy, images, static page sections
- [ ] 🟡 **Yellow** — Routes, forms, SEO metadata, dependencies, build config, navigation, inline-CSS cleanup
- [ ] 🔴 **Red** — DNS, payments, login, database, secrets, GitHub Actions, deployment settings (do NOT auto-merge; requires explicit owner approval)

## Files changed

<!-- List the files and briefly state purpose of each change -->

| File | Change |
|------|--------|
|      |        |

## Security self-check

- [ ] No new dependencies added (or new dependency justified below)
- [ ] No `.github/`, deployment, DNS, env, or config changes (or justified below)
- [ ] No external scripts, CDNs, iframes, analytics, tracking added
- [ ] No API keys, tokens, `.env` content, or other secrets in code/logs/screenshots
- [ ] No customer info, internal URLs, private emails, or unpublished references
- [ ] No `dangerouslySetInnerHTML`, `eval`, `new Function`, or `innerHTML` usage
- [ ] Mobile and desktop both render correctly (for user-facing changes)
- [ ] Both Japanese and English versions updated (if applicable to repo)

## Verification

- [ ] Local preview tested
- [ ] Owner physical iPhone test (required for any mobile-affecting change)
- [ ] Cascade diff review (required for any Yellow/Red change)
- [ ] Comet independent review (recommended for Yellow/Red, required for Red)
- [ ] No console errors

## Rollback plan

<!-- How to revert this PR if issues are detected after merge -->
<!-- For most PRs: `git revert <merge-commit>` is sufficient -->

## Related

<!-- Link to AUDIT or ENGAGEMENT-REPORT documents if applicable -->
<!-- Link to Devin work summary if relevant -->
