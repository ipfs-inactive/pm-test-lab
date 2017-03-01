#IPTL Sprint Objectives for the 2/27/2017 sprint

#HackMD notes 2/28/2017:
https://hackmd.io/KwBgjGBsCcCG0FoCm8BMCAsAOD6BGAxgMxgKwbTTADsIAJtdakUA

#Sprint members
- @sidharder
- @lgierth
- @jonnycrunch
- @FrankPetrelli
- @hsanjuan
- @jbenet
- @dgrisham

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

##Documentation

- Create a guide that lets someone deploy the Kubernetes master with out config. (ideally this is very short)
- Create a guide that lets someone setup a lablet (VM and native, covered separately)
- Create a guide for issuing jobs to the lab, and other random things (CLIs).
- Create a guide for writing good tests for the lab
