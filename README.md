# Fury Kubernetes Jenkins

## Jenkins Packages

Following packages are included in Fury Kubernetes Jenkins katalog:

- [jenkins-server](katalog/jenkins-server): Jenkins is a free and
open source automation server. Jenkins helps to automate the
non-human part of the software development process, with continuous
integration and facilitating technical aspects of continuous delivery. Version: **LTS**
- [jenkins-agent](katalog/jenkins-agent): An agent is typically a machine,
or container, which connects to a Jenkins master and executes tasks
when directed by the master. Version: **3.27-1-**

Following packages are included in Fury Kubernetes Jenkins roles:

- [jenkins-agent](roles/jenkins-agent): Ansible role used to deploy Jenkins agents
in virtual machines *(debian based)*. Default version: **3.26**.


## Compatibility

| Module Version / Kubernetes Version | 1.14.X             | 1.15.X             | 1.16.X             |
|-------------------------------------|:------------------:|:------------------:|:------------------:|
| v1.0.0                              |                    |                    |                    |

- :white_check_mark: Compatible
- :warning: Has issues
- :x: Incompatible
