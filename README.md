# custom-containerized-jenkins
A recipe to customize Jenkins with commonly used plugins and build a docker image out of it 


`Learning Resources for DevOps, SRE, Cloud & Engineering Management`

[![BINPIPE](https://img.shields.io/badge/BINPIPE-YouTube-red)](https://www.youtube.com/channel/UCPTgt4Wo0MAnuzNEEZlk90A)
[![Learn DevOps!](https://img.shields.io/badge/BINPIPE-Learn--DevOps-orange)](https://github.com/BINPIPE/resources/blob/master/devops-lesson-plans.md)
[![BINPIPE](https://img.shields.io/badge/Live--Classroom-blue)](https://forms.gle/tDJxDyj2nJyfsgsk7)
---


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

<pre>
<a href="https://www.binpipe.org">BINPIPE</a> aims to simplify learning for those who are looking to make a foothold in the industry.
Write to me at <b>nixgurus@gmail.com</b> if you are looking for tailor-made training sessions.
For self-study resources look around in this repository, <a href="https://www.binpipe.org/">the Binpipe Blog</a> and <a href="https://www.youtube.com/channel/UCPTgt4Wo0MAnuzNEEZlk90A">Youtube Channel</a>.
</pre>

___
:ledger: Maintainer: **[Prasanjit Singh](https://www.linkedin.com/in/prasanjit-singh)** | **www.binpipe.org**
___

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
