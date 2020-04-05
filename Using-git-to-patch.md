# How to use git to patch chia-blockchian

Everyone once in a while in our pre-mainnet software, an issue will come up that's not worth generating a whole new release but is worth putting a fix in and letting interested folks apply it. This assumes you already have a working copy of chia-blockchain.

1. From your chia-blockchain directory type `git fetch` and then `git status`. You'll get back something that looks like this though sometimes you'll get much less:
```bash
# Results of 'git fetch':
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (34/34), done.
remote: Total 60 (delta 34), reused 36 (delta 26), pack-reused 0
Unpacking objects: 100% (60/60), done.
From https://github.com/Chia-Network/chia-blockchain
   b077a7a..1212590  master                 -> origin/master
 * [new branch]      beta-1.1               -> origin/beta-1.1
 * [new branch]      rk-remove-setproctitle -> origin/rk-remove-setproctitle

# Results of 'git status':
On branch master
Your branch is behind 'origin/master' by 4 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

2. Now you should stop your node/farmer with `chia-stop-all` and exit the virtual environment with `deactivate`.

3. Update your software with `git pull`. You'll get something like:
```bash
Updating b077a7a..1212590
Fast-forward
 INSTALL.md                     | 4 ++--
 README.md                      | 2 +-
 src/full_node/blockchain.py    | 3 +++
 src/full_node/full_node.py     | 3 +--
 src/wallet/websocket_server.py | 2 +-
 5 files changed, 8 insertions(+), 6 deletions(-)
```
4. Now run the install.sh file with `sh install.sh`.

5. Once that completes successfully, re-enter the virtual environment with `. ./activate` and then start whatever is appropriate for you like `chia-start-farmer &`.

6. Note that your configuration directory may have changed. If you run `ls ~/.chia` and you see more than one directory you may have to `chia-stop-all`, `rm -rf ~/.chia NEWDIR/*` where NEWDIR is the new directory created and then `mv ~/.chia/OLDDIR/* ~/.chia NEWDIR/*` as the newer install is going to look for it's configuration, plots, and plots.yaml and keys.yaml in the NEWDIR. Once you're pointed at the correct directory, `start-chia-farmer ^`. We will be adding a script to do this for you soon. NEWDIR may look like `beta-1.0b2.dev6` and OLDDIR may look like `beta-1.0b1` so the mv command above would be `mv ~/.chia/beta-1.0b1/* ~/.chia beta-1.0b2.dev6/*`. You may not want to move/mv plots though so you can just move the 'config/' and 'db/' directories. Moving config looks like `mv ~/.chia/beta-1.0b1/config ~/.chia beta-1.0b2.dev6/` and the same for the `db/' directory.

7. An alternative to moving the directories around is to type `echo CHIA_ROOT=~/.chia/OLDDIR` where OLDDIR is the initial configuration where your configuration is. You will want to add `echo CHIA_ROOT=~/.chia/OLDDIR` to your `~/.bashrc` or `~/.zshrc` so that it's there when you log in again.