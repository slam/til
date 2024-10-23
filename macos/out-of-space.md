# What to do when you run out of space on macOS

## 1. Check disk usage on common culprits

Run this command to identify large directories (using `/Library` as an example):

```sh
sudo du -h -d 2 /Library | sort -hr | head -n 30
```

Sample output:

```sh
57G     /Library
41G     /Library/Developer
37G     /Library/Developer/CoreSimulator
14G     /Library/Application Support
6.9G    /Library/Application Support/Adobe
3.3G    /Library/Developer/CommandLineTools
...
```

Common locations to check for large files:

- `/Library`
- `~/Library/Caches`
- `~/Library/Application Support`

## 2. Delete unnecessary files

Note: Exercise caution when deleting files from system directories. While it's generally safe to delete files from `~/Library/Caches`, always research the specific files or folders before removal to avoid unintended consequences.

## 3. Dealing with persistent space issues

If you're still running out of space after deleting files, Time Machine snapshots might be the culprit. Refer to the TIL on this topic for more information.
