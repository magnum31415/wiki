# Remote-SSH

## VS Code Remote-SSH hacia VM (multipass)

Consigue la IP de tu VM

````bash
multipass info dev-vm
````

### Configura acceso SSH a la VM

Desde tu Mac, genera una clave si no tienes una:

````bash
ssh-keygen -t ed25519
````

### Copia la clave pública dentro de la VM:

````bash
multipass shell dev-vm
mkdir -p ~/.ssh && echo "TU_CLAVE_PUBLICA_AQUI" >> ~/.ssh/authorized_keys
````

Conecta desde VS Code

Cmd+Shift+P → "Remote-SSH: Connect to Host" → añade ubuntu@IP_DE_TU_VM.


## Instala la extensión correcta

Abre el panel de Extensiones: Cmd + Shift + X
Busca exactamente: Remote - SSH
Instala la que es de Microsoft (autor: ms-vscode-remote),

o más rápido ````ext install ms-vscode-remote.remote-ssh````

## Conecta desde VS Code

Cmd+Shift+P → "Remote-SSH: Connect to Host" → añade ubuntu@IP_DE_TU_VM.
