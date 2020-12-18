# GIT

Tipy i inne rzeczy

## Cheat sheet

- [Log](https://elijahmanor.com/blog/git-log)
- [Branch](https://elijahmanor.com/blog/git-branch)
- [Github](assets/other/git/github-git-cheat.pdf)
- [Atlasian](assets/other/git/atlassian-git-cheat.pdf)

## ohmyzsh aliases

Aliases for common git commands in [ohmyzsh docs](https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet#git)

## Weź jeden plik z innego brancha

```
git restore --source NAZWA_BRANCHA -- PATH_DO_PLIKU
```

np.
```
git restore --source experiment -- app.js
```

## Auto upstream
Nie pisz za każdym razem
`git branch --set-upstream remote/...`

```
git config --global push.default current
```

## Commiter vs Author

Commiter - osoba zapisana w ustaawieniach (globalnych lub dla danego repozytorium)
```
git config user.name "user name"
git config user.email "user@example.com"
```

Author - domyślnie brane z commitera, może być nadpisane używająć `--author`
```
git commit -a -m "some changes" --author "Mr. Random <other-user@example.com>"
```

Wtedy commiter będzie `user@example.com`, a author `other-user@example.com`.

> You may be wondering what the difference is between author and committer. The author is the person who originally wrote the patch, whereas the committer is the person who last applied the patch. So, if you send in a patch to a project and one of the core members applies the patch, both of you get credit — you as the author and the core member as the committer.

Czyli jest to dla repo, które używają maili do dodawania commitów (np. kernel linuxa)

[Source](https://stackoverflow.com/questions/18750808/difference-between-author-and-committer-in-git)