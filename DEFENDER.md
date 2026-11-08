# If Windows Defender flags DynoHash

DynoHash is a GPU miner. Malware that hides on other people's computers mines coins too, so antivirus tools watch mining
software closely. Windows Defender sometimes flags a new DynoHash build as `Trojan:Win32/Bearfoos.A!ml`. The `!ml` ending
means a machine-learning model made the call, not a signature written for DynoHash. It usually happens while the miner is
connecting to a pool. Each new build is a new file to Defender, so a build that was fine yesterday can be flagged today, and
the next build may not be.

We do not ask you to turn Defender off, and DynoHash never does it either.

## 1. Check that your copy is ours

Only do the next steps with a file you downloaded from https://dynohash.com/download or
https://github.com/dynohash/dynohash/releases.

- Every release file is listed in `SHA256SUMS`, and that list is signed with our minisign key (key ID `187DA934220B370A`,
  published on the download page and in the GitHub README).
- In PowerShell: `Get-FileHash .\DynoHash_<version>_x64-setup.exe` and compare the hash with the line in `SHA256SUMS`.
- With minisign: `minisign -Vm SHA256SUMS -P <our public key>`.

If the hash does not match, delete the file. It is not ours.

## 2. Tell Microsoft it is a false positive

This is what clears the detection for everyone, usually within a few days.

1. Go to https://www.microsoft.com/en-us/wdsi/filesubmission and choose **Home customer**.
2. Product: **Microsoft Defender Antivirus**. Upload the flagged file (`dynohash.exe` from the install folder, or the
   installer). If Defender already removed it, upload the installer you downloaded.
3. Detection name: the one Windows Security shows (for example `Trojan:Win32/Bearfoos.A!ml`).
4. Choose **Incorrectly detected as malware/malicious** and add: "DynoHash GPU miner for the Pearl coin from dynohash.com,
   installed on purpose. SHA-256 matches the signed SHA256SUMS."

We report every flagged release ourselves as the developer, but a report from a user helps too.

When Microsoft has reviewed the file, get the new definitions: Windows Security > Virus & threat protection >
Protection updates > **Check for updates**, or `Update-MpSignature` in PowerShell.

## 3. Get the file back

After the new definitions are in, or once you have checked the file in step 1:

1. Open **Windows Security > Virus & threat protection > Protection history**.
2. Open the DynoHash entry (it names `...\AppData\Local\DynoHash\dynohash.exe`).
3. Choose **Actions > Restore**.

If the file is gone from the history, run the DynoHash installer again (your settings are kept).

## 4. Keep Defender from removing it again (an exclusion for the DynoHash folder only)

If Defender removes it again before Microsoft has answered, you can tell Defender to leave the DynoHash folder alone.
Most GPU miners need this from time to time. Only do it after step 1 showed your copy is ours, and only for this one
folder:

1. Open **Windows Security > Virus & threat protection > Manage settings** (under "Virus & threat protection settings").
2. Scroll to **Exclusions > Add or remove exclusions > Add an exclusion > Folder**.
3. Pick `C:\Users\<you>\AppData\Local\DynoHash` (the folder DynoHash installed to).
4. Restore the file as in step 3, or run the installer again.

Or, in PowerShell run as administrator: `Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\DynoHash"`

Keep the exclusion to that folder: never exclude a whole drive, your Downloads folder, or a file type, and never turn
Defender off. Defender keeps protecting everything else. Updates DynoHash installs itself are signature-checked before
they run. To remove the exclusion later: the same Exclusions page, or
`Remove-MpPreference -ExclusionPath "$env:LOCALAPPDATA\DynoHash"`.

## 5. Tell us

Write to support@dynohash.com with the detection name, the DynoHash version (`dynohash --version`), the pool you were
connecting to, and the time. It helps us find what the model reacts to.

## What DynoHash does to look like what it is

- `dynohash.exe` and the app carry their publisher (DynoHash), a description and version in Properties > Details, and an
  application manifest that runs with your own rights and never asks for administrator rights.
- It installs per user, starts only when you start it (or at sign-in if you leave "Start with the computer" on, which you
  can turn off in Settings), and shows its window, its tray icon and its log.
- Your payout address is read from a file, never put on a command line.
- The 2% dev fee is shown in the app, the README and the log.
- Release files are signed with minisign. They do not have a Windows (Authenticode) code-signing certificate.
