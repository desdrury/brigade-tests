# brigade-tests

[Install](https://github.com/Azure/brigade/blob/806a37f5b6c2aa8305dc9deca3f2bb94fd5823eb/docs/intro/install.md).

```console
$ helm repo add brigade https://azure.github.io/brigade

# Install
$ helm install brigade/brigade --name brigade-server --namespace default

# Possibly only needed on an RBAC enabled cluster
$ k create clusterrolebinding brigade --clusterrole=cluster-admin --serviceaccount=default:brigade-brigade-ctrl

# Upgrade
$ helm upgrade brigade-server brigade/brigade
```

Projects, builds, jobs.

```console

$ helm install brigade/brigade-project -n desdrury-brigade-tests -f brigade-projects/brigade-tests/values.yaml  --namespace default

$ brig project list

# Show projects using Helm, sorted by release date (newest first)
$ helm list brigade -d

# Upgrade Brigade project to latest version of Helm Chart brigade/brigade-project
$ helm upgrade desdrury-brigade --reuse-values brigade/brigade-project

# This will use the brigade.js in the Git repo
$ brig run desdrury/brigade-tests

# The most recent project is at the bottom of the list
$ brig build list

# The worker.id contains the name of the Pod that ran the worker
$ brig build get 01bzxb3y07eq8bdqjs5n3m7nx9

# Show the build logs from the worker
$ k logs brigade-worker-01bzxb3y07eq8bdqjs5n3m7nx9-master

# Run a build file using an existing project
$ cd ~/Sites/ThirdParty/brigade/docs/topics/examples/
$ brig run desdrury/brigade-tests -f brigade-11.js

# Show builds
$ brig build list
$ k get po,secret -a -l component=build

# Show jobs for a build
$ k get po,secret -a -l component=job,build=01bzxcj9d7p0nr9nff7dp8mt1p

# Clean up old builds and jobs
$ k delete po,secret -l component=build
$ k delete po,secret -l component=job
```