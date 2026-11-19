#GIT

## Git Commands
### Config

````
git config --global user.name ' Username'
git config --global user.email student@domain.com
git config --global push.default simple
git config --global  credential.https://git.lab.domain.com.username student
git config --global  credential.helper cache --timeout=7200
git config --global -l
````
### Create new branch

Creem la branca ````git checkout -b develop````

verifiquem que hem canviar de branca ````git branch````

pujem la nova branca creada a l'origin ````git push origin develop````


### Delete branch

delete local: ````git branch -d nombre_rama````

delete remote: ````git push origin --delete nombre_rama````

### Merge branch

1. Estem a la branca DEVELOP i mirem les diferències amb MMASTER ````git diff master````

2. Canviem a la branca MASTER ````git checkout master````

3. Validem que estem a MASTER ````git branch````

4. Consultem les diferències amb DEVELOP ````gif diff develop````

4. Fer el merge de DEVELOP cap a MASTER ````git merge develop````

5. Validem que ja no hi ha cap diferència entre les branques ````gif diff develop````

6. Pujem els canvis ````git push origin master````

7. Actualitzem CHANGELOG amb la nova versió 1.0.1 ````vim CHANGELOG.md````

````
## [v12-1.0.2] - 10-10-2023
### FIXES
- [TAGS] merge branch v12.tag.1 to v12 to fix issue to avoid to remove tags
````

8. Verifiquem fitxers modificats: ````git status````
   
9. Afegim al stage el fitxer ````git add CHANGELOG.md````

10. Fem el commit indicant nova versió ````git commit -m "Ver 1.0.1"````

11. Pujem els canvis ````git push origin master````

12. Creem nou tag x la versió que hem creat ````git tag -a 1.0.1````

13. Pujem el tag  ````git push origin 1.0.1````


## configure  prompt

Ro configure your prompt to automatically update to indicate the status of your working tree. The easiest way to do it is to use the git-prompt.sh script that ships with the git package. Add the following lines to your ~/.bashrc file.

````
source /usr/share/git-core/contrib/completion/git-prompt.sh
export GIT_PS1_SHOWDIRTYSTATE=true
export GIT_PS1_SHOWUNTRACKEDFILES=true
export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '
````
