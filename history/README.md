# history

The record of how this repository's history was extracted from the monorepo,
[The Test Cabinet](https://github.com/TheClockwyrks/TheTestCabinet), where the
crates here lived until the extraction. Nothing builds or tests against these
files; they are kept so the provenance outlives the scratch clone the
extraction ran in.

## The extraction

The history below the scaffold commit is the monorepo's `nightly` at
`94eb8a0d5eaaabf19ab4517666eca5735488aa35`, taken from the branch
`issue/repos-6` forked from it, filtered to the paths in
[`paths.txt`](paths.txt) by git filter-repo 2.47.0:

```sh
git clone -q --no-local --single-branch --branch issue/repos-6 --no-tags \
  <the-test-cabinet> x
cd x
uvx --from git-filter-repo==2.47.0 git-filter-repo --force \
  --paths-from-file ../paths.txt --prune-empty always --preserve-commit-hashes
```

Of the 6,988 commits reachable from that commit, 13 touched a path in the list
and were kept (4 of them merges); the rest were pruned. No tag was carried. The
tip tree of every moved directory hashes the same as the monorepo's at that
commit:

```text
crates/particle-core/  f4fe531e3304a83a2791c9e35e4052bd90743af8
crates/particle-2d/    e5f83ec63be3aecf485b3d1c028a8ade84fb6b60
crates/particle-3d/    523378912a0cd4939b2321687baa270ac8bde4f8
```

The paths are kept where they were in the monorepo, so the filter needed no
rename and the path dependency of `particle-2d` and `particle-3d` on
`particle-core` works as it did.

## The files

- [`paths.txt`](paths.txt) is the list handed to `--paths-from-file`
  (sha256 `0131b5cd8cbe9138db66d889aee2b09327c973ccf6f29d46c95a5ec68660eb8a`).
  It is generated, not hand-written: the three moved directories
  (`crates/particle-core/`, `crates/particle-2d/`, `crates/particle-3d/`), and
  every path a tracked file under them had before a rename or copy, collected
  by `git log --follow -M -C --name-status` over each of the 16 files. No
  rename or copy was found, so the list is those three directories.
- [`commit-map`](commit-map) is filter-repo's map from each monorepo commit (left)
  to the commit it became here (right), all zeros for a pruned one.
  `--preserve-commit-hashes` left the commit ids quoted in the messages as they
  were, so an id in a message below the scaffold commit names a monorepo commit,
  and this map translates it.

## The live-path review

The list names no path outside the three moved directories, so no live file of
the monorepo came along and the scaffold commit deletes nothing.
