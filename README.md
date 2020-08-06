## ansible for Blue Green deployment

Ansible playbooks for configure the VMs, build imags and deploy with blue/green technique.


### configure VMs

run this command:

> ansible-playbook -i hosts configureVM/InitialSetup.yml

this playbook will install required packages and docker, create user and upgrade and restert VM.


### build and push docker image

run this command:

> ansible-playbook -i hosts buildDockerImage/build.yml

check **buildDockerImage/vars/default.yml** before run.

this playbook pull code repository with git on build host. find your latest tag and bulid docker image with it. use **git tag** for **image tag**. at last push image to docker hub. 

### Blue Green Deployment

playbook is base on [this](https://github.com/decayofmind/ansible-bluegreen-docker) playbook. run this command:

> ansible-playbook -i hosts deployBlueGreen/main.yml

read **deployBlueGreen/Variables.md** and check **deployBlueGreen/vars/default.yml** before run.

this playbook get latest tag name from docker hub, cleanup vm, set the logic of blue green deployment, run docker container and perform healthcheck on that. after all generate nginx config for app, if deployment was successful send **fininsh** message on telegram and when if deployment was failed send **failure** message on telegram.

#### roll back to pervious version

run this command:

> ansible-playbook -i hosts deployBlueGreen/rollingBack.yml

this playbook use logic of  *Blue Green Deployment* playbook. 









