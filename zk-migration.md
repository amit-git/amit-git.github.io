# Live migration guide

## Problem

You have existing mesos framework setup that is running tasks on your mesos agents. You need to migrate your existing zookeeper cluster to a new one. That means you are looking to do a live migration as your production tasks are in flight.

## Solution

You need to stand up a separate stack consisting of

* Mesos masters
* Zookeeper cluster
* Mesos agents

But you can't just stand up new clusters and delete the old ones. Because that would just crash all the tasks that are currently running on the mesos agents. It means you are going to have some way of draining those tasks out as you deploy new clusters.

### Step 1 - Deploy Zookeeper cluster-B and run a continuous sync program to copy paths between the two.
### Step 2 - Deploy Mesos Master Cluster-B that is connected to Zookeeper Cluster-B
Nodes in Mesos Master Cluster-B will join Mesos Master Cluster-A since they both are publishing to the same Zookeeper path prefix and paths are getting bidirectionally copied between Zookeeper Cluster-A and Zookeeper Cluster-B.
### Step 3 - Tear down Mesos Master Cluster-A
Once you have both mesos master clusters logically merged, you don't need Mesos master cluster-A. Leader from Mesos Master Cluster-B can serve as the mesos master of Agent Cluster-A.
### Step 4 - Deploy Mesos Agent Cluster-B
Mesos Agent Cluster-B is connected to Zookeeper Cluster-B and will be the cluster that will take on new mesos tasks as we continue to drain tasks from Mesos Agent Cluster-A.
### Step 5 - Tear down Mesos Agent Cluster-A
Once all tasks are drained from Mesos Agent Cluster-A, we can safely tear it down.
