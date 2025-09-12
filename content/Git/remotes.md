

INICIAR EL REPO LUEGO PONER ESTO
git add . &&
git commit -m "second commit from backend" &&
git branch -M main &&
git remote add origin https://github.com/CrHacker7/hospitalRegister.git &&
git push -u origin main

subir al remoto una rama creada en local sin nada
`git push -u origin devops`
`git config pull.rebase false` ==(solution with no ancestor)==

### Manage remotes

$ git remote -v
# View current remotes
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)
> destination  https://github.com/FORKER/REPOSITORY.git (fetch)
> destination  https://github.com/FORKER/REPOSITORY.git (push)

$ git remote rm destination
# Remove remote
$ git remote -v
# Verify it's gone
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)

