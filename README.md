# study-libpod
The study on libpod library

## Create the testing environment
```
gcloud compute instances create demo-podman \
      --image-family=rhel-8 --image-project=rhel-cloud \
      --zone=us-central1-a

gcloud compute ssh demo-podman --zone=us-central1-a 
gcloud compute start demo-podman --zone=us-central1-a 
gcloud compute stop demo-podman --zone=us-central1-a 
```

## Installation
```
sudo dnf -y update
sudo systemctl reboot
sudo dnf module list | grep container-tools
sudo dnf install -y @container-tools  
```

## Abstract
Podman (CLI) , LIBPOD (Libraries) -> Container Management, Image, (Kernal namespace cgroup management)
1. can simple use alias docker=podman
    it work on docker run, and docker images etcs
1. it can run as rootless process.

1. Checked on the project podman-docker-compose
    https://github.com/containers/podman-compose
    The docker-compose work, by building a multi-containers within a pod, and using localhost to communicate. 
1. supports both OCI and docker images. Buildah, a command-line alternative to writing Dockerfiles and build OCI image. Tried to config to use custom nexus.
		https://podman.io/whatis.html
		https://www.padok.fr/en/blog/container-docker-oci
1. replaced the role of "containerd" not "dockerd", dockerd is the thing that helps you work with volumes, networking or even orchestration. And of course it can launch containers or manage images as well, but containerd is listening on linux socket and this is just translated to calls to its GRPC API.
1. For the part of orchestration, padman has roadmap to integrate with cri-o (Container runtime interface Openshift), it is a daemon to launch OCI-Runtime
1. Orchestration parts, is still under development, for example 
...podman generate kube  => print this is under development
...podman play kube => not support deployment and replicaset types

6. Both "containerd and podman use runc"

## Commands
podman run -dt -p 8000:80 --name nginx-pod nginx:latest

podman generate kube nginx-pod  > nginx-pod.yml

podman stop 

podman play kube nginx-pod.yml

## Reference URLs
https://github.com/containers/libpod/blob/master/docs/tutorials/rootless_tutorial.md
https://github.com/containers/libpod/blob/master/rootless.md
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
https://github.com/containers/libpod/issues/4093
