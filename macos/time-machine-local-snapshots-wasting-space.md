# Freeing Space When `df -h` Doesn't Reflect Deletions: Time Machine's Local Snapshots

I deleted GBs of data from `/Library/Caches` and `/Library/Application Support`, but `df -h` didn't show any change. The culprit? Time Machine's local snapshots.

## Viewing Local Snapshots

To see local snapshots:

```sh
sudo tmutil listlocalsnapshots /
```

## Thinning Local Snapshots

If you find old snapshots, thin them:

```sh
sudo tmutil thinlocalsnapshots / 9999999999999999 4
```

This command thins local Time Machine snapshots:

- `sudo`: Run with admin privileges
- `tmutil thinlocalsnapshots`: Thin local snapshots
- `/`: Apply to root directory
- `9999999999999999`: Set a high purge amount (in bytes)
- `4`: Aggressive purging level (1-4, 4 being most aggressive)

Use this method when simply deleting files doesn't free up space.

## Persistent Snapshots

In my case, some snapshots remained after thinning:

```sh
tmutil listlocalsnapshots /
Snapshots for disk /:
com.apple.TimeMachine.2024-09-11-161058.local
com.apple.TimeMachine.2024-09-11-191155.local
com.apple.TimeMachine.2024-09-11-231233.local
```

I couldn't delete these normally.

## Solution: Safe Boot Mode

The solution was to boot into "safe boot" mode and delete the snapshots from there:

```sh
tmutil deletelocalsnapshots 2024-09-11-161058
Deleted local snapshot '2024-09-11-161058'
```

To delete all snapshots:

```sh
for d in $(tmutil listlocalsnapshotdates | grep "-"); do sudo tmutil deletelocalsnapshots $d; done
```

Result: Disk space usage dropped from 95% to 69%.

```sh
df -h .
Filesystem      Size  Used Avail Use% Mounted on
/dev/disk3s1s1  927G  633G  294G  69% /
```

## How to Boot into Safe Mode on Apple Silicon Macs

1. Shut down the Mac.
2. Press and hold the power button until startup options appear.
3. Select the boot disk.
4. Hold down the Shift key. The "Continue" button changes to "Continue in Safe Mode".
