# GIT

Tipy i inne rzeczy

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