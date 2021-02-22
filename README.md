## Sample and Advanced Deployment with Jenkins under Minikube

### History

2021-22-02 - 1.0.1: code update
2021-22-02 - 1.0.0: initial release

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

To start Minikube and mount folders, see below. My Minikube cluster is running under Linux. If you are using Virtualbox with minikube, /Users for Mac or /hosthome for Linux are automatically mounted. In my case, I run a Minikube cluster under Linux.

```
minikube start --memory=8096 --driver=virtualbox --kubernetes-version=v1.20.0 --mount=true --mount-string="/hosthome/christian/Work/Projets/kubernetes/Volumes/jenkins_home:/data/jenkins_home/"
```

See "minikube ssh"

