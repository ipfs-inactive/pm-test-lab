Writing jobs
===

Writing jobs for the IPTL is simple; all it takes is some YAML and basic shell scripting knowledge.

Simple tests
---
Let's write a very simple test for the IPTL. This one will do the following:
- run one node
- add a small file to ipfs 
- check that getting the file from ipfs works
- remove it
- run `ipfs repo gc`

We'll start with a YAML file and walk through the configuration options

```
name: Add and GC # Just to keep things organized, name your test
config:
  nodes: 1 # The number of nodes we need running to do this test
  times: 10 # How many times to repeat all the steps
  expected: # When we make an assertion, the expected outcome should be put here. If 1 assertion is made in all the steps, and times is set to 10, put 10 under the expected outcome
      successes: 10
      failures: 0
      timeouts: 0
steps:
  - name: Add file
    on_node: 1 # Use this field to change which node we run the test on
    # cmd states what we will run
    cmd: head -c 10 /dev/urandom | base64 > /tmp/file.txt && cat /tmp/file.txt && ipfs add -q /tmp/file.txt
    # Save outputs by line number into named variables.
    outputs: 
    - line: 0
      save_to: FILE
    - line: 1
      save_to: HASH
  - name: Cat added file
    on_node: 1
    # List the variables we want to add back into this step
    inputs:
      - FILE
      - HASH
    cmd: ipfs cat $HASH
    # Make an assertion; in this case, we want to be sure that original input came out the same after adding it to IPFS.
    assertions:
    - line: 0
      should_be_equal_to: FILE
  - name: Run GC
    on_node: 1
    cmd: ipfs repo gc
```

This completes a trivial test case. With the basic operation out of the way, let's move on to some more complex test cases.

```
name: Swarm downloading a small file
config:
  nodes: 11 # Here we need a few more nodes. Note that kubernetes-ipfs will automatically scale the deployment to fit what you put here.
  times: 3
  expected:
      successes: 30
      failures: 0
      timeouts: 0
steps:
  - name: Create 5MB File
    on_node: 1
    # Create a 5MB file of text, get its md5sum, and add it to ipfs.
    cmd: head -c 5000000 /dev/urandom | base64 > /tmp/file.txt && md5sum /tmp/file.txt | cut -d ' ' -f 1 && ipfs add -q /tmp/file.txt
    timeout: 0
    outputs: 
    # Here, FILE is the MD5sum of the file.
    - line: 0
      save_to: FILE
    # HASH is the IPFS hash of the file once added.
    - line: 1
      save_to: HASH
  - name: Cat file on node 2-11
    on_node: 2
    # Here we introduce a new parameter. Using the end_node parameter will cause kubernetes-ipfs to run tests in parallel on multiple nodes. 
    # We want 10 nodes to download this file, so we'll use nodes 2-11.
    end_node: 11
    inputs:
      - HASH
    cmd: ipfs cat $HASH | md5sum | cut -d ' ' -f 1
    # Here we introduce another new parameter, timeout.
    # kubernetes-ipfs will consider a test a "timeout" if it runs for this many seconds without completion.
    timeout: 25
    assertions:
    - line: 0
      should_be_equal_to: FILE
```

Now that you can write tests, instantiate your own minikube instance using the guide in this folder to create a local test lab and begin writing tests!

Once you're confident in your design, submit a pull request to the kubernetes-ipfs project to add your test to the tests directory and we'll run it across the cluster.
