# Changelog

Every release is listed here, newest first.
Downloads are in the [README](README.md#download) and on the [releases page](https://github.com/dynohash/dynohash/releases).

## 0.1.9 (2026-11-19)

A fix release on top of 0.1.8.

- **No card is locked out any more.** In 0.1.8, if the miner was ended hard (a power cut, a restart, a forced kill) twice
  while a card was running its start-up self-test, DynoHash stopped starting that card at every later start, until an
  update. That rule is gone. A card whose self-test fails, cannot open the card, or does not finish simply does not mine for
  that run, one line in the log says why, and the next start tests it again. Cards that 0.1.8 had locked out mine again on
  the first start of 0.1.9: the old record is removed by itself, and the log says so once.
- **A lost card is called a lost card.** When the driver loses a card (for example one that drops off the bus), the log now
  says that the driver lost the card and that a restart of the computer brings it back, instead of blaming DynoHash's kernel
  or saying the card "is not stable at stock settings". The card check that DynoHash sends (when speed stats are on) gives
  this case and a self-test that did not finish their own short codes, and it now carries the NVIDIA driver version
  whenever the driver can report it, not only for cards DynoHash was tuning.
- **HiveOS / mmpOS (`--os-oc`): DynoHash's own leftover settings are no longer taken for the rig's.** If a dyno pull or a
  tuned run was ended hard, its power limit and clock offset could stay on a card, and a later start without tuning could
  then make DynoHash treat them as the rig OS's own overclock and keep them. Now only a run that read and put back a card's
  rig settings marks them as clean, so the next tuning start (or `dynohash restore`) puts such a leftover back.
- The HiveOS start script's comment now says what it really does (it starts the miner as a child process; Hive's stop still
  reaches it).
- Crash logs and `dynohash diag-bundle` remove the computer name in more cases: also where it is written several times in a
  row, or right next to other letters or digits.

## 0.1.8 (2026-11-15)

0.1.8 also carries everything that was ready for 0.1.7, which was not released on its own.

- **Press Start and it mines, within seconds.** DynoHash no longer tests kernels or runs a dyno pull before the first share
  (on a first start that took about 10 minutes, behind a screen that only said it was loading). Each card has one kernel in
  this release, the fastest we measured for its model, and nothing tries or picks another. A card you have not tuned mines on
  DynoHash's default tune for its model: that kernel at the card's own power limit, with no clock or voltage changes. The
  dyno pull is the one button for tuning and never starts by itself: the quick pull takes about 6 minutes a card (measured on
  NVIDIA cards) and the full pull about 30 minutes or more; it no longer tests kernels or
  takes a separate stock reading first: it goes through the power steps from the lowest up. On the command line, `dynohash mine`
  uses a saved tune without `--autotune`; `--autotune` still tunes a card that has no tune before it mines, and `--no-tune`
  leaves every card alone.
- **After updating from 0.1.6, run one dyno pull.** A 0.1.6 tune was made on older kernels, so 0.1.8 keeps it in the file but
  does not use it: the card mines on the default tune (on an RTX 5080 that is faster anyway: 236 TH/s at 360 W against 221
  TH/s at 378 W), the Run screen says so, and the Dyno screen marks the old tune. One dyno pull (`dynohash tune` on a command
  line or HiveOS) tunes the card for 0.1.8. The same will happen after a later update that changes a card family's kernels:
  a saved tune belongs to the kernels it was made on.
- **The undervolt never goes deeper on its own.** While it mines, DynoHash still steps a card's undervolt back at the first
  error, but it no longer tries a deeper voltage after a day without errors: only a dyno pull you start does that.
- **Hot-spot limit 110 °C on a new install** (it was 100 °C). AMD cards protect themselves at about 110 °C; at 100 °C a
  Radeon AI PRO R9700 was held to its stock power (about 130 TH/s, against 140 TH/s with 110 °C). An installed DynoHash keeps
  the limit it has (the Run screen's Fan and power panel, Hot spot). NVIDIA cards report no hot spot, so nothing changes there.
- **Cards with nearly all their memory in use.** The new NVIDIA kernels need only about 0.2 GB of card memory, so an 8 GB
  card that also drives your desktop keeps its fast kernel. If other programs hold nearly all of a card's memory, that card
  mines on a kernel that fits what is left, at the card's own settings, and the Run screen says so; close them and restart
  mining for the faster one.
- **A card whose start-up check takes the PC down is stopped.** If that happens twice in a row, DynoHash stops starting that
  card and says what to send to support, until a DynoHash update changes its kernel or a driver update brings a new CUDA
  (AMD: HIP) version.
- **Faster on NVIDIA RTX 50 cards (every one we measured).** Two new kernels since 0.1.6 build their work on the chip and move almost nothing
  through card memory; 0.1.8's own kernel also keeps everything in registers. Each card at its own power limit, every share
  checked: first new kernel against 0.1.6, then 0.1.8's kernel on top of it: RTX 5090 +17% (575 W), then +5.0% (500 W);
  RTX 5080 +4% (360 W) / +7% (280 W), then +2.0% (360 W); RTX 5070 Ti +7% (300 W), then +3.3% (250 W); RTX 5070 +0.3%,
  then +2.0 to +2.8% (250 W); RTX 5060 Ti +5%, then +2.0% (180 W); RTX 5060 +2% (145 W). RTX PRO 4000 Blackwell: +21% from
  0.1.6 to 0.1.8 (145 W). 0.1.8's kernel also uses 3 to 7% less power for the same work.
- **Faster on RTX 40: the new kernel is now their default.** RTX 4090 +5%, RTX 4080 +19% (at a 224 W limit), RTX 4070 Ti
  +1%, RTX 4070 +4%, RTX 4060 Ti +4%, RTX 4060 +3%, each at its own power limit.
- **RTX 3080, 3080 Ti and 3090: +7 to +8%** on top of 0.1.6 (3090 and 3080 Ti at 350 W, 3080 at a 300 W limit).
- **RTX 3080 / 3090 and RTX 40: a faster kernel on top of those two, now their default.** RTX 4090 +2.2% (450 W and 350 W),
  RTX 4070 +2.2% (200 W), RTX 3090 +1.9% (350 W and 280 W), RTX 3080 +3.3% (a heat-limited card), and 1 to 2% less power
  for the same work.
- **CMP 170HX and A100: a new default kernel.** CMP 170HX 64 GB +5.1% (+4.4% in a second, longer test), CMP 170HX 40 GB
  +10.2%, A100 PCIe 40 GB +19.4%, each at its host's 250 W limit, and more work per watt (1.5 times on the CMP 170HX 64 GB,
  about 1.2 times on the others).
- **RTX 3060, 3060 Ti, 3070, 3070 Ti and RTX A-series: a newer kernel by default.** In our tests on rented cards at their own
  power limits: RTX 3060 (12 GB) +6.6% (170 W); RTX 3060 Ti +8.0%, then +1.9% more on 0.1.8's newest kernel (200 W); RTX 3070
  +5.4% (220 W, a card whose clock was free); RTX 3070 Ti +4.4%, then +1.5% (310 W); RTX A4000 +12.9% (140 W); RTX A2000 12GB
  +11.5% (70 W).
- **Data-centre and workstation cards:** NVIDIA A2, A16 and A40 now use the new kernel by default (A16 +13.8% per GPU, A2
  +8.5%, A40 +8.1%); the L4 is +23% and the L40S about +22%.
- **Memory clock on NVIDIA RTX 30, 40 and 50 cards, set by itself.** Most of these cards need little memory speed for Pearl,
  so with the memory at 810 MHz the watts it no longer draws go to the chip. The first time a card mines with this version
  (and after a driver update, or when its kernel changes), a check of under two minutes compares its own speed at
  its normal memory clock and at 810 MHz at the same power limit, and keeps 810 MHz only if the card is not slower there by
  more than the measurement's noise. On rented cards at their own power limits it kept 810 MHz on the RTX 3080 (+1.5 to +2.6%,
  two cards), RTX 3090 (+3.1%), RTX 4070 Ti SUPER (+0.3%) and, within its noise and with less power, the RTX 5060 Ti, 5070 and
  5090; an RTX 4060 Ti sits at the edge at its 165 W limit (kept once, not kept once). In the miner's own bench, every share
  checked, at the card's default power limit and at a lower one: RTX 3080 +1.6% (370 W) and +4.8% (280 W), RTX 3090 +2.8% (390
  W) and +8.9% (280 W), RTX 4060 Ti +0.5% (165 W) and +3.3% (130 W), RTX 4070 Ti SUPER +0.2% (295 W) and +1.1% (230 W), RTX
  4090 +0.8% (450 W) and +2.5% (350 W), RTX 5060 Ti -0.4% (180 W, 4.0% less power) and -0.7% (150 W, 3.8% less power), RTX
  5070 -0.4% (250 W, 3.7% less power) and +0.4% (190 W), RTX 5090 -0.2% (575 W, 1.6% less power) and +1.3% (450 W). The RTX
  3060, 3060 Ti and 3070 mined about 94 to 95% slower at 810 MHz on their old kernel; on their new one the check measures
  again. Off in Settings
  ("Lower the memory speed on RTX 30/40/50 cards") or with `--mem-clock stock`. On HiveOS and mmpOS a memory clock lock or
  offset the rig OS set before DynoHash starts is left as it was set (by `mine` and `dynohash tune`). A memory clock lock set
  by anything else while DynoHash mines may be reset when it stops. On Windows the
  DynoHash helper sets it: a test build ran on one Windows PC with a real RTX card; there the memory check kept the card's
  normal clock, and mining while holding 810 MHz has not yet run on Windows. RTX 20, GTX, CMP and data-centre cards never get the automatic 810 MHz (on data-centre Ampere cards a dyno pull still
  tries memory clocks, as before). The memory goes back to normal when DynoHash stops (after a crash or a forced stop: at the
  next start or with `dynohash restore`; on Windows the helper also gives it back). A normal stop gives it back while the card
  still works; after a forced stop, or a stop before mining had started, a card that sits at 810 MHz at idle by itself (some
  desktop cards do) reads the same as held, so DynoHash says in one line that the give-back is not confirmed yet (not an
  error; the next start or the next time the card mines settles it).
- **Your own card settings come back.** If a card has a power limit or a clock / voltage offset that you set yourself in
  another tool, DynoHash now writes it down before it changes anything, says so in the log ("GPU 0: your own setting (power
  limit 378.00 W (105%)) is kept on record; DynoHash uses its own while it mines and puts yours back when it stops"), and
  puts it back when it stops: after Stop, Ctrl+C or a dyno pull. After a crash or a force-quit `dynohash restore` puts it
  back at once (the app runs it right away); a new start takes the card again and puts your setting back when that run stops.
  Before, the card was left at stock and you had to set it again. DynoHash still tunes from stock while it has the card, and
  the last line of a run or a pull ("cards are back ...") is now printed only after every card was read again. Two limits: a
  clock offset above +250 MHz, or a negative one, is not put back (DynoHash never sets one and tells you at the start); your
  power limit still is. And a power limit within half a percent of the card's default counts as the default. `dynohash
  restore` now leaves a card alone that DynoHash did not set; `dynohash restore --stock` puts every card at stock. A card
  that still carries DynoHash's own mark from a run that was force-quit (a fan floor it held, its memory clock hold) goes to
  stock: what is on it is DynoHash's leftover, not yours. Closing the miner's console window on Windows (or logging off)
  gives DynoHash about 4 seconds to put the cards back; when that is not enough it counts as a force-quit. On Windows this
  needs this version's DynoHash helper: reinstall DynoHash. With an older helper still installed DynoHash tells you at the
  start that your setting will NOT be put back. Installing or updating DynoHash puts every NVIDIA card to stock once (the
  helper restarts): set your own setting again after an install. After updating from an older version that was force-quit,
  restart the computer or run `dynohash restore --stock` once: a tune that version left on a card can otherwise be taken for
  your own setting. Confirmed on a real card so far: an NVIDIA card under Linux. The Windows path (through the helper) and
  AMD cards are covered by tests only and are not yet confirmed on a real card.
- **Stop works at once, and kernels are no longer lost.** In earlier versions a Stop during the start-up kernel test was not
  heard: the app ended the miner after 30 seconds, and the kernel being tested was left out on that card for good. 0.1.8 has
  no start-up kernel test, a Stop during a dyno pull ends it at once and mining stays stopped, and kernels that were left out
  the old way no longer matter: each card simply runs its one kernel. The app's own watchdog no longer restarts a miner in the
  middle of its start.
- **The PC stays awake while mining** and during a dyno pull (a sleep dropped the pool connection); on Linux it holds the
  system's sleep lock (a desktop's own automatic suspend has not been tried yet). Only sleep is blocked, so the screen still turns off and locks. Turn off "Keep this PC awake while mining" in Settings, or use
  `--allow-sleep` / `DYNO_ALLOW_SLEEP=1`, to let it sleep. A Linux service needs a small polkit rule (in the README.md that
  comes with the download).
- **Rigs (HiveOS, mmpOS):** to update, set the miner's Installation URL to the versioned link of the release
  (`.../releases/download/v0.1.8/dynohash-0.1.8.tar.gz`, or `dynohash-mmpos-0.1.8.tgz`) and apply; the fixed-name link does
  not update a rig.
- **TH per watt:** on the app's Run screen, at the end of the miner's status line (`| 238 W, 0.308 TH/W (1m speed)`), and in
  one line per card every 5 minutes. Only measured watts are used, never a guess.
- **Less system memory with the new kernels:** about 0.2 to 0.3 GB per card while mining (0.1.6: about 0.4 GB), with no
  rise at a job change.
- **Honest bench numbers:** `bench` and the dyno pull count exactly the passes they time. Older versions
  counted one pass more, up to about 7% high on slow cards (under 1% on most), so a 0.1.8 bench can read a little lower
  than an older one while the card mines as fast or faster. Pool speeds were always right.
- **`--os-oc` never runs a card above the rig's own power limit, to the watt**, also under the card's lowest percent step.
- The log, `bench` and `selftest-point` name the build; `bench` prints the work size the card ran.

## 0.1.6 (2026-11-11)

- **NOCK on Rellik Pool now pays.** 0.1.5 earned Pearl there but no NOCK, because that pool credits NOCK only for its own
  proof shape. With a NOCK address set, a card that has the kernel for it now mines that shape by itself: NVIDIA RTX 20 to
  RTX 50 and AMD RX 9000 (not the GTX 16 cards). It needs about 5 GB of free card memory; a card with less, or one that
  cannot start at that shape, mines Pearl at its own shape there, and the log and the status say so. The shape is chosen at
  start and kept for the run. It is slower on some cards than our own: about 12% on AMD (one R9700), and RTX 3080 / 3090
  mine it without their new kernel.
- **RTX 3080 and 3090 (and their Ti models): a new kernel.** It builds its work on the chip and moves almost nothing through
  card memory (0.3 GB on the card, 3 GB before). Same speed at the stock power limit, faster under any lower one. RTX 3090:
  +4% at 300 W, +9% at 280 W, +23% at 250 W. RTX 3080: +1 to 2% at 250-280 W, +9% at 220 W (one card of each, every share
  checked). Nothing to set: these cards use it by themselves. Other cards keep their kernel; a saved dyno pull's kernel pick
  is made again once.
- **Pearlhash is no longer in the NOCK pool list.** Its servers refuse the standard mining login, so DynoHash cannot
  mine there.
- **F2Pool** is in the pool list (log in with your F2Pool account name). A backup pool that will refuse your login (a wallet
  on F2Pool, an F2Pool account name elsewhere) is pointed out at start.
- **Fewer reconnects:** a healthy pool is no longer dropped between two jobs. A silent pool is now left after 10 minutes,
  not 60 seconds (0.1.5 lost 2 to 4% of mining time to needless reconnects).
- **Shorter pause at a job change:** the work for a new job is prepared 4 to 5 times faster and while the cards finish the
  pass in flight.
- **Less memory:** at the Rellik NOCK shape the miner keeps about 0.2 GB of system memory per card instead of 4.4 GB, and
  AMD cards use about 4.5 GB of card memory instead of 9.5 GB.

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
