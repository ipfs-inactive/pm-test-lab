Issuing jobs
===

With a running cluster, let's issue some jobs to the test lab.

Getting started
---

Make sure you have kubectl installed and connected to your kubernetes master (whether in production or on a local minikube instance)
1. Run `kubectl get nodes -owide` and make sure your nodes show up and are in ready state.
1. Download / Clone the [kubernetes-ipfs](https://github.com/ipfs/kubernetes-ipfs/) repository on your local host.
1. Run some tests! Try out `go run main.go tests/add-and-gc.yml` for a simple test, and try `go run main.go tests/swarm.yml` for a larger test.
