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
