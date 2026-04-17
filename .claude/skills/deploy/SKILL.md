---
name: deploy
description: Run lint, build production bundle, and push to staging
---

Run the full deployment pipeline for this project. Execute each step sequentially and stop on failure.

1. **Run lint** — `npm run lint`
   - If this fails, report the lint errors and stop. Do not proceed.

2. **Build production bundle** — `npm run build`
   - If this fails, report the build errors and stop. Do not proceed.

3. **Push to staging** — Push the `dist/` build output to the staging environment.
   - Run `git add dist/ && git push origin main` to push the latest build.
   - If the user specifies a different deploy method, use that instead.

After all steps succeed, summarize: what passed, the bundle size from the build output, and confirm the push to staging completed.
