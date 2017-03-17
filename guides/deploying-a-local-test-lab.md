Deploying a Protocol Labs Local Test Lab
===

Installing prerequisites
---

- Download **minikube** from [GitHub - Minikube Releases](https://github.com/kubernetes/minikube/releases)
- Download **kubectl** from [GitHub - Kubernetes Changelog](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md)
  - Make sure to download the client binary (named `kubernetes-client-{platform}.tar.gz`) for your platform
  - Extract the `kubectl` binary (`kubectl.exe` for Windows Users)
- Download VirtualBox or use QEMU/KVM if you choose

Starting minikube
---
Tweak as necessary to fit your environment
```
minikube config set cpus 4
minikube config set memory 4096 # In MB
minikube config view
minikube delete || true 
minikube start # Add --vm-driver={vmdriver} if you don't wish to use Virtualbox.
```
Minikube is now running locally!

Getting started
---
1. Run `kubectl get nodes -owide` and make sure your local minikube shows up. The minikube binary should have configured your kubectl to connect to the minikube instance.
1. Download / Clone the [kubernetes-ipfs](https://github.com/ipfs/kubernetes-ipfs/) repository on your local host.
1. Run some tests! Try out `go run main.go tests/add-and-gc.yml` for a simple test, and try `go run main.go tests/swarm.yml` for a larger test.
