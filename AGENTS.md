# AGENTS.md

## Working agreement

The user decides product behavior. Choose the implementation and checks yourself. Ask about unresolved product choices. A request authorizes its scope. Reviews and questions do not authorize edits.

Use the user's language in conversation, product documents, and tasks. Write technical documents and agent instructions in English. Follow ASD-STE100: short sentences, active voice, and consistent terms.

## Implementation

Keep business rules in their owning modules. Modules call other modules through their public API. Correct shared defects where the wrong result originates. Do not add a workaround in each caller. Change generated files through their source and generator.

## UI

Use the existing design system. Product components own colors, typography, padding, borders, and radius. Use theme tokens. Parents control placement through wrappers and gaps. Do not pass `className` or `style` into product components. Preserve keyboard and focus behavior. Show loading, empty, error, and success states for real data.

## Tests and TDD

Use test-driven development for behavior changes and bug fixes. Confirm the expected failure, implement the change, then refactor while tests pass. Copy and styling changes need no test-first development.

Prefer integration tests through real application or core interfaces. Keep internal modules and test databases real. Replace external providers with test doubles. Add unit tests only for cases that integration tests cannot cover efficiently. Do not duplicate coverage or assert UI wording, CSS, markup, or snapshots.

## Verification

Run relevant checks locally through the project's scripts. Do not add cloud CI. Report changes, checks, failures, and required user actions. Never claim a check you did not run.

Interactive browser use requires an explicit request. Run scripted browser checks only as prescribed by project instructions or requested by the user. The user performs the final visual check.

## Documentation

Code and configuration are the source of truth. Document only decisions, constraints, and procedures they do not explain. Update the owning document instead of adding guides or repeating instructions. Remove completed setup instructions.

## Git and safety

Inspect Git status and remotes before Git operations. Stage, commit, push, change branches or worktrees, or rewrite history only on explicit request. Never stash or discard another person's work. Preserve the existing remote. Do not add AI attribution to commits.

Ask before destructive actions, spending, new dependencies, or changes to data access. A request naming a dependency authorizes it. Never print or commit secrets or personal data. Never weaken security controls.

Keep temporary files in the system temp directory and delete yours afterward. Use your own ports. Stop only processes you started.

## Deployment

Deploy only on request through the project's release tools. Follow its deployment guide. Stop for a dirty tree, unpublished source, or an unclear release commit.

Release candidates stay local. Do not publish an RC to a public registry. Build the tarball and install it from the local file instead. Tag the release commit, push the tag, and keep the RC version in `package.json` so the local install reports the RC version.

```sh
npm run build
mkdir -p /tmp/oco-pack
npm pack --pack-destination /tmp/oco-pack
npm install -g /tmp/oco-pack/opencommit-<version>.tgz --registry=https://registry.npmjs.org/ --min-release-age=0
oco --version
```

Use the npmjs registry for the install. The user registry is a mirror, and the mirror refuses the tarball install. Disable `min-release-age` for the same reason. Keep the tarball in the system temp directory and delete it after the install.

Verify the install with `oco --version`. It must print the RC version from `package.json`.

## Maintenance

No slop. Write clear, declarative code. Prefer the smallest solution that meets the requirement and stays easy to read. Remove code your changes make unused.

Write as if you will maintain this code for the next 10 years. Keep it clean. Take ownership of every module you change and leave it better than you found it.
