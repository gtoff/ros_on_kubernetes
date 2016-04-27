# ROS on Kubernetes

This repository contains a simple example of how to run ROS nodes on Kubernetes.
It creates 3 pods, a master, a talker, and a listener that communicate on a ROS topic.
The whole point of the experiment was to find out the correct configuraiton of 
Kubernetes pods and services that allow ROS nodes to discover and talk to each other.

## TL;DR: What worked for us

- We use one pod per ROS node and we wanted the nodes to be able to talk to each other seamlessly
- Kubernetes services create A Records in the internal Kubernetes DNS, so we 
thought this was a good way to let nodes find each other
- It turns out that "regular" services can only be declared in association with 
one or more ports, while ROS nodes use dynamic ports and let the master do the 
matchmaking
- Luckily, Kubernetes also supports the notion of "Headless" services (http://kubernetes.io/docs/user-guide/services/#headless-services)
- With a headless service, each pod associated with a service gets its own DNS 
entry rather than the service getting a Virtual IP and relaying on Kube-Proxies 
and a specific port.
