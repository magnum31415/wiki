# Commands

#TFENV

## tfenv installation

`git clone --depth=1 https://github.com/tfutils/tfenv.git ~/.tfenv`
echo 'export PATH=$PATH:$HOME/.tfenv/bin' >> ~/.bashrc
tfenv list
tfenv install X.Y.Z
tfenvi use X.Y.Z
`

# Launch Terraform inside a Docker Container
## Creación del container
`docker build -t tf12.31 .`

## Run del Container
`docker run -d -ti -p 80:80 \
 -w /data \
 -v $PWD:/data:rw,z \
 -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
 -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
 -e AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN} \
  tf12.31`
## Login an container para lanzar el TF desde dentro
`docker exec -it -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} -e AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN} IDCONTAINER /bin/sh`
