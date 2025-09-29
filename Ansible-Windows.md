# WSL - Virtualbox - Vagrant - Ansible - Windows - WinRM

Access from WSL to virtualbox images provisiones by vagranat

## WSL - Create file **c:/Users/USERNAME/.wslconfig**
````cmd
wslconfig 
[wsl2]
# Opcional (Windows 11): red en modo "mirrored" para que localhost funcione en ambos sentidos
networkingMode=mirrored
dnsTunneling=true
autoProxy=true
firewall=true
````

## Asnsible -  inventory

Create inventory file

**HTTPS** (recomendado)  por localhost:55986

````
[windows]
backend1

[windows:vars]
ansible_connection=winrm
ansible_host=127.0.0.1
ansible_port=55986
ansible_winrm_scheme=https
ansible_winrm_transport=ntlm
ansible_user=.\vagrant
ansible_password=vagrant
ansible_winrm_server_cert_validation=ignore
ansible_winrm_read_timeout_sec=120
ansible_winrm_operation_timeout_sec=60
````

**HTTP** por localhost:55985 (solo lab)

````cmd
[windows]
backend1

[windows:vars]
ansible_connection=winrm
ansible_host=127.0.0.1
ansible_port=55985
ansible_winrm_scheme=http
ansible_winrm_transport=ntlm     # con NTLM el canal va cifrado a nivel mensaje
ansible_user=DOMINIO\\usuario
ansible_password=TuPassword
ansible_winrm_read_timeout_sec=120
ansible_winrm_operation_timeout_sec=60
````

## Asnsible -  test connection

````bash
ansible windows -i hosts.ini -m ansible.windows.win_ping -vvv
ansible windows -i hosts.ini -m ansible.windows.setup -vvv
````
