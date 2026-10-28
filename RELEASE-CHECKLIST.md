# Remastered release checklist

Owner's rule: **all changes go to the prerelease channel only.** The launcher
resolves the bundle from the newest release asset of its selected channel;
the game builds resolve from the `plus` branch of
`saintromuald/cosmonarchy-bw-prerelease` (GitHub).

## 1. Game-side changes (the normal path)

```
cd ~/Games/cosmonarchy/prefix/drive_c/Cosmonarchy/Prerelease
# edit remastered/run.sh, mpq/anim/*, mpq/images.rel ...
git add -A remastered mpq/anim mpq/images.rel
git commit -m "remastered: ..."
git push mine plus          # github saintromuald/cosmonarchy-bw-prerelease
git push gitlab plus        # personal mirror (keep in sync)
```

Machines pick this up through the launcher: Plus — Prerelease — Latest
(tracks the branch). NEVER push to `saintromuald/cosmonarchy-bw-release`
(release channel) or to any upstream the-no-frauds-club repository.

## 2. Bundle-side changes (rare: samase binaries / dispatcher logic)

1. Sync the dispatcher into the bundle source first:
   `cp ~/.local/share/cosmonarchy-remastered/run-samase.sh \
       ~/Projects/cosmonarchy-hd/tools/run-samase.sh`
   (the bundle's entry must be the THIN dispatcher; the real logic lives in
   the game repo so game updates never need a bundle re-issue).
2. Re-issue:
   `tools/publish-remastered-bundle.sh <unlock password> --prerelease`
   (ALWAYS --prerelease per the owner's rule).
3. On both machines: launcher Settings -> Remastered channel = prerelease
   (done on both machines 2026-10-28), then unlock (one time per machine).
   Unlock downloads the newest prerelease bundle.

## 3. Verification (every change)

```
tools/probe: /tmp/opencode/cosmo-launch-probe.sh <name> <seconds>
# or simply launch and check the game's remastered/samase.log:
#   "Loading custom images.rel" + "Loading custom anim/main_XXX.anim"
#   and ZERO "dummied out" lines; game reaches the login/menu.
```

## 4. Distribution of the full anim pack

The complete roster (~440 anims, 2-4 GB) does NOT go into git. Options,
in order of preference:
- (a) launcher streaming download step (new launcher version: fetch the
  pack release asset, stream-extract into the selected version's mpq/anim),
- (b) encrypted bundle (current EnsureBundle buffers everything in RAM —
  not suitable for GBs without the launcher change).

Until (a) lands, the game repo carries the curated pilot set. Current
coverage: `tools/coverage.py` emits the per-image matrix (HTML).

---

(Imported from the cosmonarchy-hd source repo; the owner's prerelease-only
rule applies to every release here: `--prerelease` always.)
