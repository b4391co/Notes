<h1 style="text-align:center;font-size:40px;">PPS </h1>

## GIT

```sh
git config user.mail "nombre@gmail.com"
git config core.editor "nvim"

git init # inicializar git en el directorio
git add .  # añadir archivo, lo mismo que:

# =

git add *.*
git commit -am "Primer commit" # añadir comentario (a add ↖) (m message)
git status
git branch # listar ramas
git diff # comprobar diferencias
git log --online --graph
git checkout master -- fichero01.c # recuperar imagen del repositorio
git clone [ruta] 
git remote
git fetch origen # preguntar cambios

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