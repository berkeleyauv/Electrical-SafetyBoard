## Cloning and First-Time Setup

1. Install [Git for Windows](https://git-scm.com/download/win) if you don't already have it.
2. Install Git LFS from [git-lfs.com](https://git-lfs.com/).
3. Clone the repo:
```powershell
   git clone https://github.com/berkeleyauv/Electrical-SafetyBoard.git
```
4. From *inside* the cloned folder, run:
```powershell
   cd Electrical-SafetyBoard
   git lfs install
```
   Running this inside the folder (not just once globally on your machine) is what wires LFS into this specific repo's hooks — skip it and large files may not get tracked through LFS correctly.
5. Open the `.PrjPcb` file directly from this cloned folder in Altium Designer. Don't save it into an Altium 365 Workspace or move it anywhere else — Altium needs to read/write files in this exact git-tracked folder.

## Daily Workflow

**Before you start working, every time:**
```powershell
git pull
```
Non-negotiable. Altium files don't merge like code — editing on top of stale data can silently overwrite a teammate's work when you push. Pulling first means you're always starting from the latest version.

**While you work:** stay in your own hierarchical sheet (Core & Power / Sensing & Cutoff / Comms-Logging-UI). If you need to touch the top-level parent schematic, say so in the group chat first — two people editing the same Altium file at once is exactly the kind of conflict git can't resolve for you.

**When you're done with a chunk of work:**
```powershell
git status
```
Check what's listed before adding anything. If you see `History/`, `__Previews/`, or anything ending in `.Lck`, stop and ask rather than commit it — something's off with your `.gitignore`. Otherwise:
```powershell
git add .
git commit -m "short description of what you changed"
git push
```

**If `git push` gets rejected:** someone else pushed since your last pull. Run `git pull` first, then push again. If `git pull` flags a conflict on an actual Altium file (not just something like the README), stop and get the team together instead of guessing — that's the one case git genuinely can't merge automatically, and picking wrong means losing someone's work.

## Common Gotchas

- Close Altium before running `git pull` or `git status` — an open project can leave `.Lck` lock files lying around.
- Double-check `.gitattributes` exists in your clone before your first commit touching `.SchDoc`/`.PcbDoc` files — if it's missing, those files get committed as regular git objects instead of through LFS.