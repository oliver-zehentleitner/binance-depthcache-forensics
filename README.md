# Binance DepthCache Forensics

Interactive forensic charts and raw audit data for a 25-hour Binance Spot DepthCache benchmark.

This repository contains the supplementary material for the article:

**Your Binance DepthCache is rotting — here's the proof in 25 hours**  
https://blog.technopathy.club/your-binance-depthcache-is-rotting-here-s-the-proof-in-25-hours

## What this is

The benchmark compares two local Binance Spot order book implementations over a 25.10-hour run on `BTCUSDT`:

- **naive** — applies Binance depth stream updates into a local book without an active retention/pruning policy.
- **fixed** — applies the same updates, but actively prunes the local book back to the maintained depth corridor.

Both variants were fed by the same WebSocket stream and audited against REST depth snapshots at regular intervals.

The goal is not to claim that Binance's synchronization procedure is broken. Binance documents snapshot initialization, update continuity, and gap recovery. The point of this benchmark is narrower:

> Correct synchronization is not enough. A local DepthCache also needs an explicit retention policy, otherwise stale streamed levels can accumulate as ghost levels.

## Live supplementary material

Open the GitHub Pages index here:

https://oliver-zehentleitner.github.io/binance-depthcache-forensics/

## Interactive charts

Main comparison:

- [comparison.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/comparison.html)

3D forensic scatter plots:

- [report_naive.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/report_naive.html) — large Plotly file
- [report_fixed.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/report_fixed.html) — large Plotly file

Additional forensic views:

- [distance_naive.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/distance_naive.html)
- [age_naive.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/age_naive.html)
- [volatility_naive.html](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/volatility_naive.html)

## Raw data

The full audit dataset is available here:

- [raw_data.tar.gz](https://oliver-zehentleitner.github.io/binance-depthcache-forensics/raw_data.tar.gz)

It includes the audit JSON files, archived REST snapshots, summary data, and the run log used to verify the benchmark numbers.

## Run summary

- Symbol: `BTCUSDT`
- Duration: `25.10 h`
- Audits: `27`
- WebSocket reconnects: `10`
- WebSocket client: `unicorn-binance-websocket-api` / UBWA `2.12.2`
- Local cache variants: `naive` and `fixed`
- Audit reference: REST depth snapshots

## Important context

Binance's Spot API documentation currently recommends using a REST depth snapshot with `limit=5000` and documents update-ID continuity checks. It also warns that levels outside the initial snapshot may not reflect the full view of the order book unless they change.

This benchmark focuses on what happens when streamed price levels are inserted into a local cache without an active retention policy. The observed failure mode is the accumulation of orphaned local price levels that are no longer present in REST snapshots.

## Related article

This benchmark follows the earlier qualitative analysis:

**Your Binance order book is wrong — here's why**  
https://blog.technopathy.club/your-binance-order-book-is-wrong-here-s-why

## License

The repository is intended as supplementary research material. Unless stated otherwise, the data and generated charts are published for inspection, verification, and discussion.
