# Live migration guide

## Problem

You have existing mesos framework setup that is running tasks on your mesos agents. You need to migrate your existing zookeeper cluster to a new one. So you are looking to do a live migration as your production tasks are in flight.

## Solution

You need to stand up a separate stack consisting of

* Mesos masters
* Framework scheduler(s)
* Zookeeper cluster
* Mesos agents
