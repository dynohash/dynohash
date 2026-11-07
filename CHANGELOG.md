# Changelog

Every release is listed here, newest first.
Downloads are in the [README](README.md#download) and on the [releases page](https://github.com/dynohash/dynohash/releases).

## 0.1.1 (2026-11-07)

- **The levels have new names and jobs.** Street is quiet (fan up to 65%, tuned for efficiency). Track is the default:
  the most profit against your power cost (fan up to 80%). Nitrous is the most stable power after tuning (no fan cap,
  power up to what the card allows).
- **If you start the miner with `--preset track`: it means something new.** In v0.1.0 `track` had no fan cap and allowed
  +10% power. Now `track` holds the fan under 80% and tunes for profit. For the old behaviour use `--preset nitrous`.
  `--preset strip` (the old default) now means `track`. The miner says this once in the log when it starts.
- `--tune-mode` now always wins when you give it. Left out, the level picks: Street efficiency, Track profit (the fastest
  stable setting when no coin rate is known), Nitrous max-hash.
- The dyno pull keeps under your temperature limit (`--temp-limit`, or the app's setting): it does not pick a power whose
  hot spot reaches it.
- While mining, power is trimmed at once when the hot spot reaches your limit. The fan is raised first only in the band
  just under the limit.
- A card running deeper than -60 mV keeps its crash record for as long as it runs there. If the PC hangs, the next start
  steps that card back two notches instead of trying the same voltage again.
- A level you edit is never deeper than what the dyno pull proved on the card. Settings shows the voltage really used, and
  says why when it is shallower than you typed.
- The app reads the current coin rate (for "per day" figures) whether speed stats are on or off. That request sends
  nothing about you; Settings > What is sent says so.
- A card you keep at stock is never lifted by Nitrous.

## 0.1.0 (2026-11-06)

- First public release: Pearl (PRL) mining, the dyno pull, time slips and The Strip.
- Windows installer and command line, Linux .deb, AppImage and command line, HiveOS and mmpOS packages.
