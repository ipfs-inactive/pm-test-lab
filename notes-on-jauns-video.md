[Juan Video](https://www.youtube.com/watch?v=giQfhypeo7g)

# InterPlanetary Lab (IPTL)
Product Requirements (Architecture and Artifacts - Version 1.0)

The inspiration for IPTL comes from the old Planet Lab which was an economic testing ground for different distributed file system. Planet Lab was successful because it was a real network.

The Goals of IPTL is to be able to have a really large network and be able to deploy arbitrary binaries and test them robustly in many different scenarios. We need to be able to configure networks that can run a set of workloads.

We are building this so we can generate extensive metrics around certain work loads. We need to be able to orchestrate different configurations of IPFS nodes. We need to be able to setup GO, IPFS, and Orbit nodes interesting ways so we can see how they will interact with each other.

## The Core Components

1. The Orchestrator
  a. The orchestrator is the lab manager.
  b. We may be able to leverage Kubernetes to perform the heavy lifting
  c. The Orchestrator will push jobs to the runners.
  d. The orchestrator will ship arbitrary processes to the runners.
  e. Kubernetes has cubelets.
2. The runners.
  a. The units that actually run the processes.
  b. Could be implemented as Kubernetes containers.
  c. Runners may run on the cloud and also on our own hardware.
  d. It's important to test on our own harder/network, this is what made Planet Lab successful.
  e. Runners accept jobs from the Orchestrator
  f. Runners must run arbitrary processes.
  g. We need to consider using VM's here.  VM's should make it much easier for the end user.
  h. The VM could automate running the process of the Kubernetes container.
  i. So the runners are VM's, let's call them lablets.

## The Orchestrator
A Link to Juan Bennet's Awesome Orchestrator page goes hear.

The orchestrator will be communicating with many runners who's state can be completely un-predictable. The orchestrator needs to be able to recognize and recover from faults.

Developers or the clients will interact directly with the orchestrator or through a series of services that are hosted along side.

There will be a lot data to move around. We should consider using IPFS to move this data around.  The benefit will be that we will save a ton in bandwidth if move all this data around in IPFS.  This will also have the side effect of deduplicating the data in the containers. To be able to use IPFS to move this data around we will need to put some effort into the IPFS Importer.

There could be a manage process which would give ssh access to a process.

## A Runner (Lablet)
Lablets will run a process that will keep talking to the orchestrator. This could just be the Kubernetes process.

A single lablet should be able to run multiple processes.

The goal will be to ship a VM that can have one or more containers.

We may have some interesting networking challenges. Allowing nodes to connect through NAT as an example.

Requirments:
1. Must be able to run abitrary programs.
2. Must have a container engine.
3. Must be a VPN
4. Must be able to connect to other Lablets Directly
5. Must have a supervisor process that captures everything.
6. Must work in Linux/Mac and windows.
7. Must be able to gather metrics.
8. Must be able to deliver those metrics to the management services/end users.
9. Must be able to recover from serious job failures. (binary shipped that eats all the memory)
10. It would be good for a reboot to occur if the VM crashes.
11. It's important to minimize the maintenace required by the people running the test.
12. Must require little to no user management
13. Must be self contained (VM)

##The Three User Sets
1. DevOps - The folks in the IPFS community who are building this.
2. The Developer - These are the people crafting the tests and shipping the tests to the test lab. These are the miners.
3. The people running the actual tests.

## DevOps
1. Setup the orchestrator/lab
  a. Should be repeatable
  b. Needs to be automated as much as possible
  c. Need to be well documented
  e. Could be provissioned on the cloud
  f. Should be up 24/7
  g. Should come with a bundle of stuff to run
  h. This may be Kubernetes by itself or it may be something added to Kubernetes
  i. Should be able to start it off and not have to worry about it.
2. Running (after setup)
  a. Automate as much as possible
  b. Should require minimal maintenace
  c. Need to watch the complexity of this, if it gets to complex we may need to consider other avenues.
  d. A dashboard can be considered that can be used to monitor statistics.
  e. Should work similar to Travis.
3. Reporting Errors
  a. When something happens the lab must go offline
  b. People need to know when they can and cannot use it.
  c. Need to have a status page.

##The Minors (the people running the lablets)
Phase 1:
1. Minors will start by preparing their hardware.
2. This can be downloading some software that we give them.
3. This should be as simple as possible.
4. In the beginning we will certainly fall short on this objective.
5. Need a document which can go with the software so the minors can figure it out on their own.
6. Things will be hard on users during this phase.
7. Need to target one architecture with a document that explains everything so that users can work their way through it. should be exhaustive.
8. Needt to generate a FAQ

Phase 2:
1. Need the perfect installer to make it as easy as possible.
2. Users should be able to get up and running quickly on their own.
3. It would be useful for the user to be able to allocate resources
4. Should be able to click a single button to go online or to start.
5. Turning the program on should take vary little work. 30 seconds tops
6. We could use VM limits at this point to restrict resources.
7. The constraints can be really useful to us as well.
8. The user should be able to check out a dash board where they can see what's going on.
9. The user should be able to stop the process very easily.
