# Commandes utilisées étape par étape:

Ref [Git > 9. Notions avancées Git > 52. Librairies pour les hooks](https://dyma.fr/developer/list/chapters/core/5e7bbf46a673090444ec2c80/lesson/git/5e7e75fa03ef627d0ff7e401/9/2)


```sh
mkdir -p husky-git-test && cd husky-git-test && echo "node_modules" > .gitignore && cat ../readme.md > readme.md
```


```sh
npm init -y
```


```sh 
git init
```


```sh
npx husky init
```


```sh
cat <<'EOF' >.husky/pre-commit
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

exec < /dev/tty
if test $(git diff --cached | grep ^+.*console.log | wc -l) != 0
then
    echo "Au moins un console.log est ajouté dans votre commit !"
    echo "Êtes-vous certain de continuer ?"
    read -p "[o/n]>" choix
    if test $choix != "o"
    then
        echo "Abandon du commit"
        exit 1
    fi
fi
EOF
```


```sh
npm install --save-dev @commitlint/{cli,config-conventional}
```


```sh
echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```


```sh
echo "npx --no -- commitlint --edit $1" > .husky/commit-msg
```


```sh
git add .
```


## Refusé
```sh
git commit -m "loremipsum___"
```
retour
```
husky - DEPRECATED

Please remove the following two lines from .husky/pre-commit:

#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

They WILL FAIL in v10.0.0

Au moins un console.log est ajouté dans votre commit !
Êtes-vous certain de continuer ?
[o/n]>o
⧗   input: loremipsum___
✖   subject may not be empty [subject-empty]
✖   type may not be empty [type-empty]

✖   found 2 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint

husky - commit-msg script failed (code 1)

```


## Accepté
```sh
git commit -m "chore: add husky library with instructions" 
```
retour
```
husky - DEPRECATED

Please remove the following two lines from .husky/pre-commit:

#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

They WILL FAIL in v10.0.0

Au moins un console.log est ajouté dans votre commit !
Êtes-vous certain de continuer ?
[o/n]>o
[master (root-commit) 360b0f6] chore: add husky library with instructions
 7 files changed, 1366 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .husky/commit-msg
 create mode 100644 .husky/pre-commit
 create mode 100644 commitlint.config.js
 create mode 100644 package-lock.json
 create mode 100644 package.json
 create mode 100644 readme.md
```

## Envoi vers repo
```sh
git remote add origin git@github.com:elearning-julien-j/husky-git-test.git
echo -e "CHECK remote: " && git remote -vv
git branch -M main
git push -u origin main
```
