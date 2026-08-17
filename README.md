# git-sizer

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/git-sizer) [![KDE Eco](https://img.shields.io/badge/KDE%20Eco-certified-brightgreen?logo=kde&logoColor=white&style=flat-square)](https://eco.kde.org/) [![Blue Angel](https://img.shields.io/badge/Blue%20Angel-DE--UZ%20215-0055a4?style=flat-square)](https://www.blauer-engel.de/en/certification/criteria) [![Energy](https://api.green-coding.io/v1/ci/badge/get?repo=Interested-Deving-1896%2Fgit-sizer&branch=main&workflow=eco-audit.yml)](https://metrics.green-coding.io/ci-index.html)


<!-- AI:start:what-it-does -->
_Description pending._
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
_Architecture documentation pending._
<!-- AI:end:architecture -->

## Install

<!-- Add installation instructions here. This section is yours — the AI will not modify it. -->

```bash
git clone https://github.com/Interested-Deving-1896/git-sizer.git
cd git-sizer
```

## Usage


By default, `git-sizer` outputs its results in tabular format. For example, let's use it to analyze [the Linux repository](https://github.com/torvalds/linux), using the `--verbose` option so that all statistics are output:

```
$ git-sizer --verbose
Processing blobs: 1652370
Processing trees: 3396199
Processing commits: 722647
Matching commits to trees: 722647
Processing annotated tags: 534
Processing references: 539
| Name                         | Value     | Level of concern               |
| ---------------------------- | --------- | ------------------------------ |
| Overall repository size      |           |                                |
| * Commits                    |           |                                |
|   * Count                    |   723 k   | *                              |
|   * Total size               |   525 MiB | **                             |
| * Trees                      |           |                                |
|   * Count                    |  3.40 M   | **                             |
|   * Total size               |  9.00 GiB | ****                           |
|   * Total tree entries       |   264 M   | *****                          |
| * Blobs                      |           |                                |
|   * Count                    |  1.65 M   | *                              |
|   * Total size               |  55.8 GiB | *****                          |
| * Annotated tags             |           |                                |
|   * Count                    |   534     |                                |
| * References                 |           |                                |
|   * Count                    |   539     |                                |
|                              |           |                                |
| Biggest objects              |           |                                |
| * Commits                    |           |                                |
|   * Maximum size         [1] |  72.7 KiB | *                              |
|   * Maximum parents      [2] |    66     | ******                         |
| * Trees                      |           |                                |
|   * Maximum entries      [3] |  1.68 k   | *                              |
| * Blobs                      |           |                                |
|   * Maximum size         [4] |  13.5 MiB | *                              |
|                              |           |                                |
| History structure            |           |                                |
| * Maximum history depth      |   136 k   |                                |
| * Maximum tag depth      [5] |     1     |                                |
|                              |           |                                |
| Biggest checkouts            |           |                                |
| * Number of directories  [6] |  4.38 k   | **                             |
| * Maximum path depth     [7] |    13     | *                              |
| * Maximum path length    [8] |   134 B   | *                              |
| * Number of files        [9] |  62.3 k   | *                              |
| * Total size of files    [9] |   747 MiB |                                |
| * Number of symlinks    [10] |    40     |                                |
| * Number of submodules       |     0     |                                |

[1]  91cc53b0c78596a73fa708cceb7313e7168bb146
[2]  2cde51fbd0f310c8a2c5f977e665c0ac3945b46d
[3]  4f86eed5893207aca2c2da86b35b38f2e1ec1fc8 (refs/heads/master:arch/arm/boot/dts)
[4]  a02b6794337286bc12c907c33d5d75537c240bd0 (refs/heads/master:drivers/gpu/drm/amd/include/asic_reg/vega10/NBIO/nbio_6_1_sh_mask.h)
[5]  5dc01c595e6c6ec9ccda4f6f69c131c0dd945f8c (refs/tags/v2.6.11)
[6]  1459754b9d9acc2ffac8525bed6691e15913c6e2 (589b754df3f37ca0a1f96fccde7f91c59266f38a^{tree})
[7]  78a269635e76ed927e17d7883f2d90313570fdbc (dae09011115133666e47c35673c0564b0a702db7^{tree})
[8]  ce5f2e31d3bdc1186041fdfd27a5ac96e728f2c5 (refs/heads/master^{tree})
[9]  532bdadc08402b7a72a4b45a2e02e5c710b7d626 (e9ef1fe312b533592e39cddc1327463c30b0ed8d^{tree})
[10] f29a5ea76884ac37e1197bef1941f62fda3f7b99 (f5308d1b83eba20e69df5e0926ba7257c8dd9074^{tree})
```

The output is a table showing the thing that was measured, its numerical value, and a rough indication of which values might be a cause for concern. In all cases, only objects that are reachable from references are included (i.e., not unreachable objects, nor objects that are reachable only from the reflogs).

The "Overall repository size" section includes repository-wide statistics about distinct objects, not including repetition. "Total size" is the sum of the sizes of the corresponding objects in their uncompressed form, measured in bytes. The overall uncompressed size of all objects is a good indication of how expensive commands like `git gc --aggressive` (and `git repack [-f|-F]` and `git pack-objects --no-reuse-delta`), `git fsck`, and `git log [-G|-S]` will be.  The uncompressed size of trees and commits is a good indication of how expensive reachability traversals will be, including clones and fetches and `git gc`.

The "Biggest objects" section provides information about the biggest single objects of each type, anywhere in the history.

In the "History structure" section, "maximum history depth" is the longest chain of commits in the history, and "maximum tag depth" reports the longest chain of annotated tags that point at other annotated tags.

The "Biggest checkouts" section is about the sizes of commits as checked out into a working copy. "Maximum path depth" is the largest number of path components for files in the working copy, and "maximum path length" is the longest path in terms of bytes. "Total size of files" is the sum of all file sizes in the single biggest commit, including multiplicities if the same file appears multiple times.

The "Value" column displays counts, using units "k" (thousand), "M" (million), "G" (billion) etc., and sizes, using units "B" (bytes), "KiB" (1024 bytes), "MiB" (1024 KiB), etc. Note that if a value overflows its counter (which should only happen for malicious repositories), the corresponding value is displayed as `∞` in tabular form, or truncated to 2³²-1 or 2⁶⁴-1 (depending on the size of the counter) in JSON mode.

The "Level of concern" column uses asterisks to indicate values that seem high compared with "typical" Git repositories. The more asterisks, the more inconvenience this aspect of your repository might be expected to cause. Exclamation points indicate values that are extremely high (i.e., equivalent to more than 30 asterisks).

The footnotes list the SHA-1s of the "biggest" objects referenced in the table, along with a more human-readable `<commit>:<path>` description of where that object is located in the repository's history. Given the name of a large object, you could, for example, type

    git cat-file -p <commit>:<path>

at the command line to view the contents of the object. (Use `--names=none` if you'd rather omit these footnotes.)

By default, only statistics above a minimal level of concern are reported. Use `--verbose` (as above) to request that all statistics be output. Use `--threshold=<value>` to suppress the reporting of statistics below a specified level of concern. (`<value>` is interpreted as a numerical value corresponding to the number of asterisks.) Use `--critical` to report only statistics with a critical level of concern (equivalent to `--threshold=30`).

If you'd like the output in machine-readable format, including exact numbers, use the `--json` option. You can use `--json-version=1` or `--json-version=2` to choose between old and new style JSON output.

To get a list of other options, run

    git-sizer -h

The Linux repository is large by most standards. As you can see, it is pushing some of Git's limits. And indeed, some Git operations on the Linux repository (e.g., `git fsck`, `git gc`) do take a while. But due to its sane structure, none of its dimensions are wildly out of proportion to the size of the code base, so the kernel project is managed successfully using Git.

Here is the non-verbose  output for one of the famous ["git bomb"](https://kate.io/blog/git-bomb/) repositories:

```
$ git-sizer
[...]
| Name                         | Value     | Level of concern               |
| ---------------------------- | --------- | ------------------------------ |
| Biggest checkouts            |           |                                |
| * Number of directories  [1] |  1.11 G   | !!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |
| * Maximum path depth     [1] |    11     | *                              |
| * Number of files        [1] |     ∞     | !!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |
| * Total size of files    [2] |  83.8 GiB | !!!!!!!!!!!!!!!!!!!!!!!!!!!!!! |

[1]  c1971b07ce6888558e2178a121804774c4201b17 (refs/heads/master^{tree})
[2]  d9513477b01825130c48c4bebed114c4b2d50401 (18ed56cbc5012117e24a603e7c072cf65d36d469^{tree})
```

This repository is mischievously constructed to have a pathological tree structure, with the same directories repeated over and over again. As a result, even though the entire repository is less than 20 kb in size, when checked out it would explode into over a billion directories containing over ten billion files. (`git-sizer` prints `∞` for the blob count because the true number has overflowed the 32-bit counter used for that field.)

## Configuration

<!-- Document configuration options here. This section is yours — the AI will not modify it. -->

## CI

<!-- AI:start:ci -->
_CI documentation pending._
<!-- AI:end:ci -->

## Mirror chain

<!-- AI:start:mirror-chain -->
This repo is maintained in [`Interested-Deving-1896/git-sizer`](https://github.com/Interested-Deving-1896/git-sizer) and mirrored through:

```
Interested-Deving-1896/git-sizer  ──►  OpenOS-Project-OSP/git-sizer  ──►  OpenOS-Project-Ecosystem-OOC/git-sizer
```

Changes flow downstream automatically via the hourly mirror chain in
[`fork-sync-all`](https://github.com/Interested-Deving-1896/fork-sync-all).
Direct commits to OSP or OOC are detected and opened as PRs back to `Interested-Deving-1896`.
<!-- AI:end:mirror-chain -->

## Contributors

<!-- AI:start:contributors -->
_Contributors pending._
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->
_Original project — no upstream influences recorded._
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
_No additional resource files found._
<!-- AI:end:resources -->

## Accessibility

<!-- AI:start:accessibility -->
This repo uses automated accessibility auditing via `check-accessibility.yml`.

Checks include: CODEOWNERS ownership coverage, README screen-reader compatibility,
WCAG 2.1 AA HTML compliance, audio overview (espeak-ng), and Braille output (liblouis).




Run the [Check Accessibility](https://github.com/Interested-Deving-1896/git-sizer/actions/workflows/check-accessibility.yml)
workflow to generate the first report and accessibility artifacts.
See [DOCS/accessibility.md](https://github.com/Interested-Deving-1896/git-sizer/blob/main/DOCS/accessibility.md) for the full reference.
<!-- AI:end:accessibility -->

## License

<!-- AI:start:license -->
[MIT](https://github.com/Interested-Deving-1896/git-sizer/blob/master/LICENSE.md) © 2026 [Interested-Deving-1896](https://github.com/Interested-Deving-1896)
<!-- AI:end:license -->
