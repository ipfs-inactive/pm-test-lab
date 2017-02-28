#IPTL Sprint Objectives for the 2/27/2017 sprint

#Sprint members


#Orchestration of processes

- Setup & Deploy a Kubernetes master to orchestrate the test lab.
- Setup & Deploy 2 Kubelets on the cloud that speak to the running master.
- Setup & Deploy 2 Kubelets on our own machines that speak to the running master.
- Create a lablet VM we can use to image cloud machines
- Create a lablet VM we can use to run in our machines (ideally is same VM)
- Test: issue a test to all the kubelets in the network
- example: downloads go-ipfs, runs it, adds something, and then counts the peers.

##Experiment with testing

- Create 5 different network tests:
- all go-ipfs, add + cat 1000 files
- all js-ipfs, add + cat 1000 files
- half go-ipfs and half js-ipfs, add + cat 1000 files
- orbit nodes chatting (all on go-ipfs)
- orbit nodes chatting + web (on go-ipfs, js-ipfs and js-ipfs-browser)
- Revive the tests from data.gov, and run them on the cluster
- Make some test that produces static grafana output
- Make some test that produces some simple trace, gather it afterward from all the nodes

##Testing Setup -- Job bundle and Results bundle

- Spec out a format for the "job bundle"
- Spec out a format for the "results bundle"
- Implement Job Bundle
- Implement Results Bundle
- Choose & Implement network test config DSL
- CLI: Dev should be able to start a job from CLI with 1 command
- WEB: Dev should be able to start a job from WEB with 1 pageload + 1 click
