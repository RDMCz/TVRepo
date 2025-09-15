# Git KPZ

## Undo `git add`

```shell
git reset
```

## Undo `git commit`

```shell
git reset HEAD~
```

## Změnit commit message

```shell
git commit --amend -m "New commit message"
```

## Zahodit necommitnuté změny

<details>
  <summary>⚠ Zobrazit</summary>
  
  ```shell
  git restore .
  ```

</details>

## Negovat pushnutý commit

```shell
git revert <commit_hash>
```

Vytvoří nový commit, který neguje změny commitu určeného pomocí hashe. Tento nový commit pak stačí pushnout. Pro negaci více commitů lze použít rozsah: `git revert <nejstarsi_hash>..<nejnovejsi_hash>`.
