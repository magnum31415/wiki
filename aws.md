# AWS

## 🧩 Install AWSCLI

````bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
````

## 🧩 Config AWSCLI

### ~/.aws/credentials
This file stores the actual AWS credentials (Access Key ID and Secret Access Key) for one or more named profiles.
Example:

````ini 
[default]
aws_access_key_id = AKIAEXAMPLE
aws_secret_access_key = abc123examplekey

[dev]
aws_access_key_id = AKIADEVKEY
aws_secret_access_key = devsecretkey
````

### ~/.aws/config
This file stores non-sensitive configuration settings, such as:

- Default region
- Output format (e.g., JSON, YAML)
- Profile settings (e.g., SSO, MFA, role assumption)

````ini
[default]
region = us-east-1
output = json

[profile dev]
region = eu-west-1
output = yaml
````

### Credentials

Si tienes acceso directo con un usuario IAM, puedes generar credenciales permanentes así:

🔹 Paso a paso:

1. Ve a la consola web: https://console.aws.amazon.com/iam
2. En el panel lateral, selecciona "Users".
3. Haz clic en tu usuario.
4. Ve a la pestaña "Security credentials".
5. En la sección "Access keys", haz clic en "Create access key".

  Guarda:
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY

🔐 Configura localmente:
````bash
aws configure
````

O manualmente en ~/.aws/credentials

````ini
[default]
aws_access_key_id = TU_ACCESS_KEY
aws_secret_access_key = TU_SECRET_KEY
region = us-east-1
````
Estas son credenciales de largo plazo. Asegúrate de protegerlas y preferiblemente usar perfiles.


## 🧩 Install Session Manager Plugin

The Session Manager Plugin is an extension for the AWS CLI that allows you to open interactive shell sessions (like SSH) into your EC2 instances using AWS Systems Manager (SSM) — without needing:

- SSH key pairs
- Open ports (like port 22)
- Bastion hosts

✅ Con Session Manager, puedes:

- Acceder a EC2 desde la CLI de AWS (usando aws ssm start-session).
- Ejecutar comandos remotos (como con ssh) sin exponer el puerto 22.
- Auditar sesiones, ya que todo se puede registrar en CloudWatch o S3.
- Trabajar con instancias en VPC privadas sin saltadores (bastion hosts).

✅ Requisitos para usarlo
El plugin de Session Manager instalado en tu máquina.

- El AWS CLI correctamente configurado.
- La instancia EC2:
  - Debe tener el agente SSM instalado y corriendo.
  - Tener asociado un IAM role con políticas de SSM (como AmazonSSMManagedInstanceCore).

It works through AWS APIs and is secured/audited via IAM, CloudTrail, and optionally CloudWatch or S3.

````bash
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
sudo dpkg -i session-manager-plugin.deb
````


## 🧩Install tfenv

Usando Git (recomendada)

````bash
# Clona el repositorio
git clone https://github.com/tfutils/tfenv.git ~/.tfenv

# Agrega tfenv al PATH
echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bashrc   # O ~/.zshrc si usas Zsh
source ~/.bashrc  # O source ~/.zshrc
````

✅ Uso básico de tfenv

**Ver versiones disponibles:**

````bash
tfenv list-remote
````

**Instalar una versión específica de Terraform:**

````bash
tfenv install 1.6.6
````

**Establecer versión global por defecto:**

````bash
tfenv use 1.6.6
````
Esto crea un archivo .terraform-version en el directorio, que Terraform leerá automáticamente.

**Ver la versión actualmente activa:**

````bash
terraform version
````


🛠️ **Verificación**

Asegúrate de que tfenv esté bien configurado con:

````bash
which terraform
````
# debería devolver algo como ~/.tfenv/versions/1.x.x/terraform


**Qué hace exactamente tfenv pin?**

Crea un archivo .terraform-version en el directorio actual.

Escribe dentro de ese archivo la versión actual de Terraform activa en tu entorno (tfenv use).

Esto indica a tfenv (y herramientas compatibles como direnv, CI, etc.) que debe usarse esa versión cuando estés en ese directorio.

✅ Ejemplo paso a paso
````bash
tfenv install 1.6.6
tfenv use 1.6.6
tfenv pin
````
🔽 Esto genera:

````bash
cat .terraform-version
# → 1.6.6
````
Ahora, cada vez que entres a ese directorio y uses terraform, tfenv activará automáticamente esa versión.

💡 ¿Cuándo usarlo?

- Para fijar versiones por proyecto, evitando que el equipo use versiones diferentes.
- Para integraciones con CI/CD, donde quieres controlar exactamente qué versión de Terraform se usa.
- Compatible con muchos editores y herramientas que detectan .terraform-version.


