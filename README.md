# custom-containerized-jenkins
A recipe to customize Jenkins with commonly used plugins and build a docker image out of it 



## Build a Custom Jenkins Container Image


Write a  `Dockerfile` as  below:

```
FROM jenkins/jenkins:alpine

ENV JENKINS_USER admin
ENV JENKINS_PASS admin

# Skip initial setup
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false


COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/plugins.txt
USER root
RUN apk add docker
RUN apk add py-pip
RUN apk add python-dev libffi-dev openssl-dev gcc libc-dev make
RUN pip install docker-compose

```

and the  `plugins.txt`

```
ace-editor
ant
antisamy-markup-formatter
branch-api
cloudbees-folder
credentials
cvs
docker
durable-task
external-monitor-job
git-client
git-server
git
github-api
github-branch-source
github
javadoc
jquery-detached
junit
ldap
mailer
matrix-auth
matrix-project
maven-plugin
metrics
pam-auth
plain-credentials
scm-api
script-security
ssh-credentials
ssh-slaves
subversion
translation
variant
windows-slaves
workflow-aggregator
workflow-api
workflow-basic-steps
workflow-cps-global-lib
workflow-cps
workflow-durable-task-step
workflow-job
workflow-multibranch
workflow-scm-step
workflow-step-api
workflow-support
favorite
token-macro
pipeline-stage-step
blueocean
blueocean-autofavorite
gitlab-plugin

```

then execute

```
sudo docker build -t custom-jenkins .

```

After the image  `custom-jenkins`  is finished, run it with

```
# use user root other than default jenkins to avoid permission problem
# use docker on the host, because there is no docker daemon in a docker container
sudo docker run --user root \
		 -v /var/run/docker.sock:/var/run/docker.sock \
		 -p 8080:8080 -p 50000:50000 custom-jenkins
```
