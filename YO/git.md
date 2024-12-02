‎ <h1 style="text-align:center;font-size:40px;">GIT</h1>

## Basico

```sh
git config user.mail "nombre@gmail.com"
git config core.editor "nvim"

git init # inicializar git en el directorio
git add .  # añadir archivo, lo mismo que:
git commit -m "commit"
git status
git branch # listar ramas
git diff # comprobar diferencias
git log --online --graph
git checkout master -- fichero01.c # recuperar imagen del repositorio
git clone [ruta] 
git remote
git fetch origin # preguntar cambios

git clone ssh:://
git remote add clase ssh://
git status
git fetch clase main
git log --oneline -–all
git merge clase/main main
git push
git switch -c rb-User-changelog

git branch -v origin/rb-User
git checkout origin/rb-User
git status
git fetch clase
git checkout release/v0.9.0
git merge origin/rb-User --no-commit
# --> editar CHANGELOG.md

git add .
git commit -m "Bash Script by XXXX "
git status
git push
```

## Crear repo dentro de otro

```sh
# en la carpeta a publicar
git init
git branch -m main
git add .
git commit -m "NEW"
git remote add origin  git@github.com:{USER}/{REPO}.git
git push --set-upstream origin main

# en el repo padre:
git submodule add git@github.com:{USER}/{REPO}.git {RUTA}
git add .
git commit -m "NEW REPO"
git status
git push

git rm --cached {RUTA} # Eliminar
```

## clonar repo dentro de otro

```sh
git clone git@github.com:{USER}/{REPO}.git
# en el repo padre:
git submodule add git@github.com:{USER}/{REPO}.git {RUTA}
git add .
git commit -m "NEW REPO"
git status
git push
```

## Commits

```
<type>(<File(s)>): <subject>
```

### Type
Must be one of the following:
- `build` Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- `ci` Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
- `docs` Documentation only changes
- `feat` A new feature
- `fix` A bug fix
- `perf` A code change that improves performance
- `refactor` A code change that neither fixes a bug nor adds a feature
- `style` Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)