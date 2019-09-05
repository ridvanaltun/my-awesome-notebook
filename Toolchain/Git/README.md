# Git

## Diff With Two Branches

```bash
git diff brach1:path/to/file branch2:path/to/file
```
## Master falındaki güncellemeyi başka bir dala aktarmak

Örneğin master dalımıza kritik bir güncelleme geldi. Merge sırasında bu işi yapmak yerine anında bu bu gelen değişikliği başka bir branch'e aktarabiliriz.

Farklı dal üstündeyken `git rebase master` diyerek master üstündeki değişikliği dalımıza alıyoruz.

## Diff Two Branches

branch-X te olan ama master da olmayan commitleri göster

`git log master..branch-X`

Eski stil `gitk` programı üstünden yapmak mümkün.
`gitk master..branch-X`
