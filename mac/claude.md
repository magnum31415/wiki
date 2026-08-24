
# Instala Claude Code dentro de la VM

Abre la terminal integrada de VS Code (Ctrl+`) — al estar en modo remoto, esta terminal ya se ejecuta dentro de Ubuntu, no en tu Mac:

````bash
curl -fsSL https://claude.ai/install.sh | bash
````

Verifica:

````bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
claude --version
````
