
# Ansible Cheat Sheet
[Ansible Cheat Sheet](https://github.com/magnum31415/wiki/blob/main/Ansible_Cheat_Sheet.pdf)

# Instalar venv ansible

```
python3 -m venv ansible_2.17
source ansible_2.17/bin/activate
pip install ansible-core==2.17
pip install pywinrm
```


## Proxychains

```
sudo apt-get install proxychains
vim /etc/proxychains.conf 

Add in [ProxyList]
socks5 12.34.11.123 1080
```

# Commandos 
```
proxychains ansible-playbook -i ../inventario.ini -l SERVERNAMEO01 playbook.yml --vault-password-file ../vault_pass.txt
```

```
ansible -i ../inventario.ini server01 -m setup -a 'filter=ansible_fqdn' 
ansible -i ../inventario.ini server01 -m setup -a 'filter=ansible_hostname' 
```

```
ansible '*.example.com, !*.lab.example.com'  -i inventory1 --list-hosts 
ansible  lb1.lab.example.com,s1.lab.example.com,db1.example.com -i inventory1  --list-hosts 
ansible '172.25.*' -i inventory1 --list-hosts 
ansible 's*' -i inventory1 --list-hosts 
ansible 'prod,172*,*lab*' -i inventory1  --list-hosts 
ansible 'db,&london' -i inventory1  --list-hosts 
```

