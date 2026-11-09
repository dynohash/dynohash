# Changelog

Every release is listed here, newest first.
Downloads are in the [README](README.md#download) and on the [releases page](https://github.com/dynohash/dynohash/releases).

## 0.1.5 (2026-11-09)

- **NOCK merge mining:** add your Nockchain address (`--merge-nock`, or Settings, NOCK address) and the pools that pay NOCK
  too give it for the same work, with nothing slowed down: Pearlhash and LuckyPool through the login, Rellik Pool through its
  merge extension. Any other pool gets your Pearl wallet only, and the log says so. The NOCK address is guarded like your
  wallet. The dev fee never uses it.
- **Backup pools** use the same pool and server picker as the main pool (or a custom address). The confirm screen names
  each pool and server. A backup that will refuse your account (a Kryptex username on a non-Kryptex pool) is pointed out.
- **Failover:** a pool that stops sending work is left after 60 s, not after 3 minutes.
- **HiveOS:** a Hive power limit added after the first start is respected. A payout-guard stop shows a message on the
  dashboard instead of a restart loop. **mmpOS:** TLS is kept, stats work on any Api port, and `--autotune` is the quick pull.
- **Two Windows users on one PC:** uninstalling for one user no longer removes the helper another user's DynoHash uses.
  The app offers to repair a missing helper.
- **Speed stats** are sent every hour. `--stats off` turns off card checks too.
- **Crash logs:** a driver error on a card DynoHash is not mining on (a disabled card, or one another program uses) is noted
  in the log but no longer counts as a DynoHash crash.
- The Example rig shows the real version and says its numbers are example data. The quick dyno pull says about 10 minutes.

## 0.1.5-beta.1 (beta, 2026-11-09)

Everything in 0.1.4, plus:

- **NOCK merge mining:** add your Nockchain address (`--merge-nock`, or Settings, NOCK address) and the pools that pay NOCK
  too give it for the same work, with nothing slowed down: Pearlhash and LuckyPool through the login, Rellik Pool through its
  merge extension. Any other pool gets your Pearl wallet only, and the log says so. The NOCK address is guarded like your
  wallet. The dev fee never uses it.
- **Backup pools** use the same pool and server picker as the main pool (or a custom address). The confirm screen names
  each pool and server. A backup that will refuse your account (a Kryptex username on a non-Kryptex pool) is pointed out.
- **Failover:** a pool that stops sending work is left after 60 s, not after 3 minutes.
- **HiveOS:** a Hive power limit added after the first start is respected. A payout-guard stop shows a message on the
  dashboard instead of a restart loop. **mmpOS:** TLS is kept, stats work on any Api port, and `--autotune` is the quick pull.
  (A beta has no rig packages: these reach HiveOS and mmpOS with the next release.)
- **Two Windows users on one PC:** uninstalling for one user no longer removes the helper another user's DynoHash uses.
  The app offers to repair a missing helper.
- **Speed stats** are sent every hour. `--stats off` turns off card checks too.
- The Example rig shows the real version and says its numbers are example data. The quick dyno pull says about 10 minutes.

## 0.1.4 (2026-11-09)

- **Backup pools:** add backup pools in Settings (or `--backup-pool` on the command line). If your pool gives no work (3 failed connections in a row, or no job for 60 s), DynoHash mines on the next one with the same account and worker, and goes back when your pool answers again. Your pool's other regional servers are added on their own (`--no-regional-backup` turns that off). The Run screen shows which server is in use. HiveOS: extra Pool URL lines in the flight sheet are backups; mmpOS: a repeated pool is a backup.
- **Payout guard** covers the backup pools you name, the same way as your main pool.
- **More NVIDIA cards:** GTX 16 series, CMP 30HX, T400-T2000 and Tesla V100 now mine with a DP4A kernel (in our tests a GTX 1660 Ti does about 8.7 TH/s, a V100 about 26). Faster defaults for the CMP 170HX and A100 and for the H100.
- **CMP 170HX:** a card that looks locked gets one line in the log and a note on its card in the app, with a link to the [unlock guide](https://dynohash.com/guides/cmp-170hx/). DynoHash does not unlock cards.
- **Dev fee:** if a fee server gives no work, the fee connects to another one (Kryptex's other servers, then HeroMiners), and your cards mine for you while it waits instead of sitting idle. The fee itself is unchanged.
- **Crash logs:** a computer or user name that appears several times in a row is now always removed from a log before it is sent (it could slip through once).

## 0.1.3 (2026-11-09)

- **NVIDIA tuning:** the dyno pull now tunes NVIDIA cards too: power limit and core clock boost, every share still checked on your CPU. On our RTX 2080 a pull as a normal Windows user went from 47.9 TH/s at stock to 61.5 TH/s at 171 W. On Windows a small DynoHash Helper service (installed once, with one admin prompt) sets the clocks and power for your account only.
- **Faster NVIDIA kernels:** new defaults for RTX 30, 40 and 50 (on our test cards an RTX 4090 at 450 W does about 296 TH/s, an RTX 5090 at 575 W about 360) and a faster GTX 10 kernel. GTX 10 cards get the clock boost only if the card accepts it; otherwise power limit only.
- **Watchdog:** if the miner stops answering, DynoHash restarts it; if the app and the miner are both gone, the helper starts the app again. A card that keeps failing is stepped back and then mined at stock. Optional: restart the PC when a card falls off the bus (off by default, at most 3 times a day). On HiveOS, Hive's own watchdog stays in charge.
- **Tune all cards:** one button on the Dyno screen tunes each card in turn while the others keep mining.
- **Built-in GPUs and cards DynoHash cannot control:** listed on the Dyno screen with the reason, and a kernel check finds their fastest kernel without changing any settings. NVIDIA cards show watts, temperatures, fan and clocks on Linux and Windows.
- **HiveOS:** the AMD runtime is in the package, so AMD rigs need nothing extra; the AMD warning shows only on AMD rigs. Hive's overclock is a ceiling the tune stays under and comes back when DynoHash stops, also after a crash. The first autotune is the quick pull and shows its speed in Hive.
- **Diagnostic logging and crash logs** from the 0.1.2 betas: Settings > Diagnostic logging (Normal by default), `diag-bundle` makes one cleaned zip of the logs to send us, and "Send crash logs to help fix problems" is off unless you turn it on.
- Windows Defender sometimes flags a new miner build. If it does, [here is what to do](https://github.com/dynohash/dynohash/blob/main/DEFENDER.md).

## 0.1.2-beta.3 (beta, 2026-11-08)

Everything in 0.1.2-beta.2, plus:

- **NVIDIA tuning:** the dyno pull now tunes NVIDIA cards too: power limit and core clock boost, every share still checked on your CPU. On our RTX 2080 a normal-user Windows pull went from 47.9 TH/s at stock to 61.5 TH/s at 171 W. On Windows a small DynoHash Helper service (installed once, with one admin prompt) sets the clocks and power for your account only; nothing else can use it.
- **Watchdog:** if the miner stops answering, DynoHash restarts it; if the app and the miner are both gone, the helper starts the app again. A card that keeps failing is stepped back and then mined at stock. Optional: restart the PC when a card falls off the bus (off by default, at most 3 times a day). On HiveOS, Hive's own watchdog stays in charge.
- **Tune all cards:** one button on the Dyno screen tunes each card in turn while the others keep mining.
- **Built-in GPUs and cards DynoHash cannot control:** listed on the Dyno screen with the reason, and a kernel check finds their fastest kernel without changing any settings. NVIDIA cards show watts, temperatures, fan and clocks on Linux and Windows.
- **Faster kernels:** new defaults for RTX 30, 40 and 50 and a faster GTX 10 kernel (a GTX 1080 Ti went from 12.2 to 15.7 TH/s at 250 W in our test). GTX 10 cards get the clock boost only if the card accepts it; otherwise power limit only.
- **HiveOS (in the next release's HiveOS package; a beta has none):** the AMD runtime is in the package, so AMD rigs need nothing extra; the AMD warning shows only on AMD rigs. Hive's overclock is a ceiling the tune stays under and comes back when DynoHash stops, also after a crash. The first autotune is the quick pull and shows its speed in Hive.
- Beta error reports now carry each card's own shares and clocks.

## 0.1.2-beta.2 (beta, 2026-11-08)

Same as 0.1.2 with extra logging to help us fix problems; sends error reports, never your wallet.

- **Faster on RTX 50 and RTX 40:** new default kernels. An RTX 5080 at its stock 360 W did about 207 TH/s in our test.
- **Diagnostic logging (on in the beta):** card readings, GPU driver and PCIe errors and the miner's log, kept on your PC.
  Settings > Diagnostic logging: Normal, Full (much more detail, at most about 2 GB on disk) or Off.
- **`diag-bundle`** makes one zip of those logs to email us, cleaned first: no wallet, pool login or password, user or
  computer name, or IP address.
- **Send crash logs to help fix problems** (Settings, off unless you turn it on): when a card crashes or hangs, that cleaned
  log is sent to us, at most 3 a day, stored privately and deleted after 30 days.
- The beta's error reports and speed stats work as in 0.1.2-beta.1.

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
