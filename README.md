The original tutorial for this makefile can be found here: http://engineering.riotgames.com/news/building-jenkins-inside-ephemeral-docker-container

The original repo: https://github.com/maxfields2000/dockerjenkins_tutorial/tree/master/tutorial_07

## Changes

* Slaves use only for control pipeline and run docker containers for build
* Need install patched docker-workflow-plugin: https://github.com/dins-heisenbug-2016/docker-workflow-plugin