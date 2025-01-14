# Benchmarks

## Table of Contents

- [Benchmark Results](#benchmark-results)
  - [crawl-speed](#crawl-speed)

## Benchmark Results

```sh
----------------------
linux ubuntu-latest
2-core CPU
7 GB of RAM memory
14 GB of SSD disk space
-----------------------

Test url: `https://rsseau.fr`

15 pages
```

We enable the `jemalloc` feature for the Rust spider crate. All test are done locally for test except C[wget]
since the performance changes drastically due to dns implementation used. We want to keep the
benchmarks close to a real world scenario without being impacted by drastic latency issues.

### crawl-speed

runs with 10 samples:

|                                          | `libraries`           |
| :--------------------------------------- | :-------------------- |
| **`Rust[spider]: crawl 10 samples`**     | `1 s` (✅ **1.00x**)  |
| **`Go[crolly]: crawl 10 samples`**       | `24 s` (✅ **1.00x**) |
| **`Node.js[crawler]: crawl 10 samples`** | `38 s` (✅ **1.00x**) |
| **`C[wget]: crawl 10 samples`**          | `80 s` (✅ **1.00x**) |

The concurrent benchmarks are averaged across 10 individual runs for 10 concurrent crawls with 10 sample counts.

In order for us to get better metrics we need to test the concurrency and simultaneous runs with a larger website and favorably a website that can spin up inside the local container so latency is not being tracked. The multi-threaded crawling capabilities shines bright the larger the website.
Currently even with a small website this package still runs faster than the top OSS crawlers to date.

_Note_: Nodejs concurrency heavily impacts each additional run.


### Hyperfine

Apple M1 Max (64gb)

179 pages 0.243s with latency (Jan 29, 2023).

Test url: `https://rsseau.fr`.

```
Benchmark 1: spider --domain https://rsseau.fr crawl
  Time (mean ± σ):      4.311 s ±  0.263 s    [User: 0.354 s, System: 0.139 s]
  Range (min … max):    4.102 s …  4.951 s    10 runs
```