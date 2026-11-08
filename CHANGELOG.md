# Changelog

Every release is listed here, newest first.
Downloads are in the [README](README.md#download) and on the [releases page](https://github.com/dynohash/dynohash/releases).

## 0.1.2 (2026-11-08)

- **Much faster on NVIDIA RTX 30, 40 and 50:** new default kernels for these cards. On our test cards: RTX 3080 about +48%, RTX 4070 SUPER +21%, RTX 4090 +22%, RTX 5080 +24% (204 vs 165 TH/s at 360 W).
- **11 new pools in the pool list:** 2Miners, AlphaPool, PearlFortune, LuckyPool, JetskiPool, Minerlab, Rabid Pool, Linkrra, Skypool, Suprnova and Rellik Pool, beside Kryptex, HeroMiners and K1Pool. DynoHash still never picks a pool for you.
- **Plain proofs for pools that do not use compressed ones.** A pool that does not confirm gzip proofs now gets plain proofs instead of being refused, so pools such as Suprnova work. Every share is still checked on your CPU before it is sent.
- **Pools that want the login first** (AlphaPool) now connect: DynoHash reconnects and sends the login before anything else.
- **One payout line per pool** in the pool picker, taken from the pool's own page. Pool fees are no longer shown; each pool's page has them.
- **Track has no fan limit now:** it goes for the most profit, and your temperature limit still protects the card. Street keeps its 65% fan limit. A Track level you never edited drops the old 80% limit; a fan limit you set yourself is kept.
- **The dyno chart's power axis reads low to high** (70, 80, 90, 100, 110). Stock is still measured first.
- **New AMD kernels:** the Radeon AI PRO R9700 runs a persistent-workgroup kernel, and the Radeon 780M a wider RDNA3 kernel.
- **The dyno pull ranks Street and Track at the voltage offset they actually run at**, not at stock voltage.
- **Windows Defender help:** dynohash.exe now names its publisher and version in Properties > Details. If Defender flags it anyway, [here is what to do](https://github.com/dynohash/dynohash/blob/main/DEFENDER.md).
- Card checks now tell a crash from a wrong answer. Fixes and more fuzz testing of what the miner reads from pools and servers.

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
