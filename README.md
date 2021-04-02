## Sample and Advanced Deployment with Jenkins under Minikube

### History

2021-03-03 - 1.0.2: update Jenkins to 2.263.4-lts

2021-22-02 - 1.0.1: code update

2021-22-02 - 1.0.0: initial release


![Jenkins backend](https://raw.githubusercontent.com/christi4n/minikube-jenkins/master/assets/jenkins-backend-overview.png)

### Jenkins yaml files definitions

You'll find enclosed two directories:

- one for a basic deployment
- one for a more advanced deployment

Needless to say that the most interesting parts is included in the second folder.
There are many files. The filenames are prepended with a number which indicates the order you have to proceed for the kubectl apply command.

### Technical information

The default namespace is "jenkins".

To get the password:

```
kubectl --namespace=jenkins exec --stdin --tty jenkins-75d5b7fb7b-nxmz9 -- /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

ReadWriteOnce -- the volume can be mounted as read-write by a single node
ReadOnlyMany -- the volume can be mounted read-only by many nodes
ReadWriteMany -- the volume can be mounted as read-write by many nodes

Check that the directory is correcly mounted:

```
kubectl exec --namespace=jenkins --stdin --tty jenkins-655f99d864-mxxd4 -- /bin/bash
```

To start Minikube and mount folders, see below. My Minikube cluster is running under Linux. If you are using Virtualbox with minikube, /Users for Mac or /hosthome for Linux are automatically mounted. In my case, I run a Minikube cluster under Linux. You also have to set the environnement variable for Docker so Minikube can access it from inside.

```
minikube start --memory=8096 --driver=virtualbox --kubernetes-version=v1.20.0 --mount=true --mount-string="/hosthome/christian/Work/Projets/kubernetes/Volumes/jenkins_home:/data/jenkins_home/"

eval $(minikube docker-env)
```

This command should be launched only once. The next time, just use "minikube start".

See "minikube ssh"

### Access

### Update deployment

In case you need to update the jenkins image used for your deployment, you may use the following syntax:

```
kubectl set image deployments jenkins jenkins=jenkins/jenkins:2.263.4-lts --namespace=jenkins
kubectl rollout status deployments jenkins
```

### Particular cases

#### Create a jenkins configuration for Cypress.io

You can optimize the configuration by creating dedicated volumes for Cypress and npm caches.

```
minikube start --memory=8096 --driver=virtualbox --kubernetes-version=v1.20.0 --mount=true --mount-string="/hosthome/christian/Work/Projets/kubernetes/Volumes/jenkins_home:/data/jenkins_home/"
```

You can also mount minikube this way:

```
minikube mount /home/christian/k8s/Volumes/npm_cache/:/var/cache/npm_cache
```

As we need to run npm commands, you should ensure that NodeJs is available. To verify that's ok, you have to go to Manage Jenkins > Manage Plugins and install NodeJS.
Then, go to the "Global Tool Configuration" section.

Add NodeJs with the following information:
- name: node
- install automatically: checked

Select your version.


