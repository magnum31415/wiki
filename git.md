# Git Commands

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


# configure  prompt

Ro configure your prompt to automatically update to indicate the status of your working tree. The easiest way to do it is to use the git-prompt.sh script that ships with the git package. Add the following lines to your ~/.bashrc file.

````
source /usr/share/git-core/contrib/completion/git-prompt.sh
export GIT_PS1_SHOWDIRTYSTATE=true
export GIT_PS1_SHOWUNTRACKEDFILES=true
export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '
````
