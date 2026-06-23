# Code Organization Review Status

Objective: compare Feynman's code and product organization against Claude Code, OpenAI Codex, OpenCode, Hermes Agent, and Hugging Face ML Intern, then apply only the structural/runtime improvements that keep Feynman a simple, potent AI researcher.

Confidence: 100/100 for the code shipped in this change.

Current state:
- Feynman worktree is staged for one final commit and `main` is ahead of `origin/main` by 3 existing commits before that commit.
- Deep-research skill was loaded and the external comparison repos were cloned into `/Users/advaitpaliwal/Companion/Code/_agent-research/feynman-code-organization/`.
- Current comparison commits were refreshed with `git fetch --all --prune` on 2026-06-23:
  - OpenAI Codex: `63f8f547c0b95800f3c61ee172a2a833b75faf3f`, `63f8f54 2026-06-23T12:18:25+02:00 Stop persisting bridged log events (#29599)`.
  - Claude Code: `12281998d8c85813c4b5952ed9367784aae37d31`, `1228199 2026-06-22T20:37:27Z chore: Update CHANGELOG.md and feed.xml`.
  - OpenCode: `d568fed0956bd97a5c89eae996122a8862151f6f`, `d568fed 2026-06-23T10:41:05Z fix(app): tighten mobile utility UI (#32799)`.
  - Hermes Agent: `5ecf3bf0e0726b8b33682bb5c3aad9679b7b5be4`, `5ecf3bf 2026-06-23T14:44:12+05:30 fix(slack): report ext-matched audio mimetype for rerouted voice clips`.
- Added Hugging Face ML Intern to the comparison set:
  - ML Intern: `550a209701701e6a9ac7cac70b8dbd508822d467`, `550a209 2026-06-18T14:01:26+02:00 [codex] Switch defaults to GLM 5.2 (#333)`.
- Applied one focused Feynman code-organization improvement:
  - Added `scripts/check-architecture.mjs`.
  - Added `npm run architecture:check`.
  - Verified it passes while naming known architecture debt and warning zones.
- Fixed the fresh-install Pi metadata bug found in Daytona:
  - `src/index.ts` patches Pi runtime package metadata before dynamically loading CLI code.
  - `src/pi/runtime-patches.ts`, `scripts/patch-embedded-pi.mjs`, and `scripts/prepare-runtime-workspace.mjs` now keep `@earendil-works/pi-coding-agent` and legacy aliases on Feynman's `.feynman` config directory.
  - `npm test` runs the embedded Pi patch first, so source/test flows match installed CLI behavior.
- Fixed the runtime packaging speed/OOM failure:
  - `scripts/prepare-runtime-workspace.mjs` installs current Pi packages once and links legacy `@mariozechner/*` aliases to the audited `@earendil-works/*` packages.
  - This removes duplicate Pi dependency trees from the vendored runtime workspace while preserving compatibility for Pi extensions that still import legacy namespaces.
  - Daytona `npm pack --dry-run` completed the vendored runtime preparation in 27 seconds instead of being killed with status 137.
- Final clean-room Daytona verification passed from a fresh Linux clone with the full patch applied:
  - `npm ci`
  - `npm run architecture:check`
  - `npm test` (`323/323`)
  - `npm run typecheck`
  - `npm run build`
  - `npm audit --omit=dev`
  - `git diff --check`
  - `npm --prefix website ci`
  - website `npm run lint`, `npm run typecheck`, `npm run build`, and `npm audit --omit=dev`
  - rendered docs internal-link check (`33` HTML files, `0` missing internal links)
  - `npm pack --dry-run` (`133` files, `50.9 MB` package, shasum `85d92d0cffb5f01296a0599b45ac93b6b2771b62`)
  - `.feynman/npm/node_modules/@mariozechner/*` verified as symlinks to `.feynman/npm/node_modules/@earendil-works/*`.

Deferred decisions:
- Do not add a `feynman plugins` product command yet. The next defensible plugin work is a manifest validator after PaperRank has stable `papers`, `rank`, and `artifacts` module boundaries.
- Do not ship an MCP server yet. It needs stable domain contracts first; otherwise it freezes the current PaperRank god-file shape as public API.
- Do not merge Yo-Yo's branch as-is. Only a research-core "lab canon" concept should come back later, and only when it improves evidence ranking or PaperRank methodology judgment.
- Next applied step remains mechanical PaperRank extraction, followed by CLI command splitting, then a `ResearchRun`/research-recipe artifact contract.

Source coverage:
- Local Feynman repo: read repo contract, package/runtime docs, command wiring, Pi launch path, bundled extension entrypoint, large file inventory, PaperRank symbol map, Pi docs/source installed at `@earendil-works/pi-coding-agent@0.79.10`.
- Prior Feynman memory: read relevant product-strategy lines that established PaperRank/evidence graph as the center and the "features fight for life" scope rule.
- External repos: cloned/read OpenAI Codex, Claude Code, OpenCode, Hermes Agent, and Hugging Face ML Intern.
- External docs/issues/releases: primary repo docs/source read; issue/PR history only sampled through latest commit metadata and local repo docs, not exhausted.

Next action:
- Commit and push this staged release candidate. After that, land a mechanical module extraction around PaperRank and CLI commands, then add `ResearchRun`/research-plugin contracts only after the core data contracts are explicit.
