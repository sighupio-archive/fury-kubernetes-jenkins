# Jenkins
These manifests will deploy a Jenkins master as a `Deployment` backed by a
`8Gib` volume in the `jenkins` namespace. Along the deployment, a `Service` is
created pointing to ports 8080 (Jenkins HTTP) and 50000 (Jenkins Slave
Protocol).

## Deploy
```bash
$ kustomize build . | kubectl apply -f -
```

## Access endpoint
If no ingress is configured, Jenkins can be accessed via the following command:
```bash
$ kubectl port-forward svc/jenkins 8080 --namespace=jenkins
```
afterwards it will be available at
[http://localhost:8080](http://localhost:8080).

## Authentication
Default user and password are:
```bash
user: admin
pwd: admin
```
To change the defaults, define a `kustomization.yaml` overlay with the following
content:
```yaml
secretGenerator:
  - name: jenkins
    behavior: replace
    commands:
      jenkins-admin-password: "/bin/echo -n foo"
      jenkins-admin-user: "/bin/echo -n 123456"
```

# Plugins
The following plugins are installed by default:
```bash
amazon-ecr:1.6
ansicolor:0.5.3
aws-java-sdk:1.11.403
blueocean:1.9.0
bouncycastle-api:2.17
build-blocker-plugin:1.7.3
build-environment:1.6
cloudbees-folder:6.6
credentials-binding:1.17
credentials:2.1.18
docker-build-publish:1.3.2
docker-build-step:2.2
docker-workflow:1.17
ec2:1.41
extra-columns:1.20
git:3.9.1
github:1.29.3
greenballs:1.15
icon-shim:2.0.3
kubernetes:1.13.5
ldap:1.20
mailer:1.22
metrics:4.0.2.2
pam-auth:1.4
pipeline-utility-steps:2.2.0
prometheus:2.0.0
rebuild:1.29
simple-theme-plugin:0.5.1
slack:2.3
workflow-aggregator:2.6
```
additional plugins can be installed through the Jenkins web interface and will
be persisted across deployments.

The plugin list is stored in a `ConfigMap` built from `conf/plugins.txt`.

If you need to overwrite this list, define a `kustomization.yaml` overlay with
the following content:
```yaml
configMapGenerator:
  - name: jenkins
    behavior: merge
    files:
      - plugins.txt
```

# Configuration
The default configuration is stored in a `ConfigMap` built from the following
files:
* `conf/config.xml`,
* `con/jenkins.CLI.xml`.

If you need to modify the default configuration, define a **new** `config.xml`
and a `kustomization.yaml` overlay with the following content:
```yaml
configMapGenerator:
  - name: jenkins
    behavior: merge
    files:
      - config.xml
```

# External URL
Jenkins external URL can be set by creating a
`jenkins.model.JenkinsLocationConfiguration.xml` with the following content:
```xml
<?xml version='1.1' encoding='UTF-8'?>
<jenkins.model.JenkinsLocationConfiguration>
  <adminAddress>admin@jenkins.local</adminAddress>
  <jenkinsUrl>http://jenkins.local</jenkinsUrl>
</jenkins.model.JenkinsLocationConfiguration>
```
and then defining a `kustomization.yaml` overlay:
```yaml
configMapGenerator:
  - name: jenkins
    behavior: merge
    files:
      - jenkins.model.JenkinsLocationConfiguration.xml
```
