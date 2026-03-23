Title: Projects Under `repositories/` MUST Be Git Submodules

ID: REPO-020
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-02

When:
- The agent creates or adds a project under `repositories/`.
- The agent is asked to track/version projects under `repositories/`.

Rule:
- MUST treat each top-level directory under `repositories/` as an independent git repository.
- MUST track `repositories/<project>` in the parent `clawspace` repository using git submodules (gitlink + `.gitmodules`).
- MUST NOT add regular files from inside `repositories/<project>/` to the parent repository index.
- SHOULD use an explicit SSH host alias in submodule URLs to avoid identity confusion (e.g., `git@github.com-lkcy33:ORG/REPO.git`).
- SHOULD ensure each submodule has a configured remote and can be cloned on a new machine.
- MAY add helper documentation, but MUST keep the rules source-of-truth in this pack.

Rationale:
- Each project in `repositories/` is a standalone repo with its own history, remotes, and dependencies.
- Submodules keep the parent repo clean and reduce the risk of committing sensitive files (e.g., `.env`).

Examples:
- Add new submodule:
  - `git submodule add <repo-url> repositories/<project>`
  - commit `.gitmodules` + the gitlink pointer
- Clone + init:
  - `git submodule update --init --recursive`

Failure Modes:
- Accidentally staged files under `repositories/<project>/` in the parent repo -> unstage them and keep only the submodule pointer.
- Submodule URL uses the wrong identity -> switch to the correct SSH host alias and re-sync.

Related:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- `workspace/TOOLS.md`
