# site-generator-speed

A faster drop-in replacement for makefun-site 1.0.0, built for the make.fun
bounty "Build a static site faster" (site-generator-speed-01-run-20261122).

[![make.fun bounty](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/rungkad1111/site-generator-speed/rank.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/rungkad1111/site-generator-speed/reward.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)

`solve.js` is a dependency-free ES module whose `run(input)` returns, for every
case, exactly the output tree makefun-site 1.0.0's build returns — every page's
index.html, the index and tag pages, build-report.txt and each static file,
byte for byte. `main.ts` is the harness driver: it reads every case from
/input/cases.json, answers each with `run`, and writes /output/answers.json.

How it is faster, while answering identically:

- Inline Markdown is rendered by one native scan for the next special
  character; plain spans are appended untouched and closers are found with
  indexOf plus a backslash-parity test.
- The block lexer dispatches on each line's first character, so no regex runs
  on a line that cannot match it.
- slugify has an ASCII fast path with a per-run memo; non-ASCII input keeps
  the reference's exact NFKD path.
- Templates render into an array joined once per render, each {% for %}
  reuses one scope-vars object, and link resolution is memoised per build.
- Word counting for reading time is a single hand scan with exact
  JavaScript \s semantics, replacing a global-regex match.

Verified locally with the competition kit: the practice set passes 3/3 cases
identical to the reference, and the build runs roughly 3x faster than the
house baseline's run() on the practice set (about 1.8x measured over the
whole driver process, JSON I/O included).

## License

MIT — see LICENSE.
