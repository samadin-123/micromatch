# Evaluation Setup

This file is outside the editable surface. It defines how results are judged. Agents cannot modify the evaluator or the scoring logic — the evaluation is the trust boundary.

Consider defining more than one evaluation criterion. Optimizing for a single number makes it easy to overfit and silently break other things. A secondary metric or sanity check helps keep the process honest.

eval_cores: 1
eval_memory_gb: 1.0
prereq_command:

## Setup

Install dependencies and prepare the evaluation environment.

```bash
npm install
cd bench && npm install && cd ..
```

This project is a pure JavaScript library with no build step. The main implementation is in `index.js`.

Dependencies:
- Node.js >= 8.6
- npm packages: braces, picomatch (runtime); mocha, minimatch, benchmark (dev)

The test suite has 1954 tests covering glob matching, extglobs, braces, negation patterns, POSIX classes, and edge cases.

## Run command

```bash
node bench/eval.js
```

## Output format

The benchmark must print `METRIC=<number>` to stdout.

The eval.js script measures average ops/sec across 8 common micromatch operations:
- makeRe with various patterns (star, globstar, extensions, paths, braces)
- isMatch with simple and nested patterns
- match with arrays

## Metric parsing

The CLI looks for `METRIC=<number>` or `ops_per_sec=<number>` in the output.

## Ground truth

Baseline metric represents the average operations per second across 8 representative micromatch operations. The current baseline is approximately 1,370,000 ops/sec, measured on the unmodified v4.0.8 codebase. Higher values indicate better performance.
