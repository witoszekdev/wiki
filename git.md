# GIT

Tipy i inne rzeczy

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