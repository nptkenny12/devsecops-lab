## Bonus: History Rewrite

### Before

git log -p | grep -c 'ghp_'
0

### After

git log -p | grep -c 'REDACTED'
2

git log --oneline
e82df20 (HEAD -> main) docs: add usage notes
1e366c4 feat: empty log
016589f feat: add config
96b81eb init

### The two-step pattern in real life

1. `git filter-repo --replace-text replacements.txt` — rewrite locally
2. **Rotate/revoke the exposed credential** — rewriting history removes the copy from Git, but it does not make the leaked credential safe to use.

### Two real-world gotchas discovered

1. `git filter-repo` refuses to rewrite a repository that has an existing remote unless it is a fresh clone or the command is explicitly forced. I had to treat the sandbox as disposable and verify the remote state before rewriting.
2. The rewrite changes commit IDs because every affected commit gets new content and ancestry. Any rewritten branch already published remotely therefore requires a coordinated force-push, and collaborators must re-clone or reset their local copies.
