# Create new branch
git checkout -b develop

#verifiquem que hem canviar de branca
git branch

#pujem la nova branca creada a l'origin
git push origin develop

# configure  prompt

Ro configure your prompt to automatically update to indicate the status of your working tree. The easiest way to do it is to use the git-prompt.sh script that ships with the git package. Add the following lines to your ~/.bashrc file.

````
source /usr/share/git-core/contrib/completion/git-prompt.sh
export GIT_PS1_SHOWDIRTYSTATE=true
export GIT_PS1_SHOWUNTRACKEDFILES=true
export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '
````
