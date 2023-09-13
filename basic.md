# Basic Of K8s

## Let's start with a node

- node is the smallest unit of computing  
hardware in a Kubernetes cluster. A node is
a single machine where your applications run.

- It could be a physical server in a data
center or a virtual machine in a public  
cloud like AWS or Azure. You can even build
Kubernetes from multiple Raspberry Pis.

- the unique characteristics of a single server in
your cluster, like its memory or CPU capacity.
Instead, you can let Kubernetes decide
where to deploy your service based on the  

- specifications you provide. Also, if something
happens to a single node, it can be easily  
replaced. Kubernetes will automatically
take care of redistributing the load.

- While it can sometimes be useful to work with
individual servers, that's not a Kuberentes  

- Many nodes are grouped into something called
a node pool. When you deploy an application,  

- Kubernetes will inspet each node
and choose one based on things like  
available CPU and memory capacity.
If that node fails for some reason,  
Kubernetes will make sure your
application is moved and kept running.

- You can have different groups of nodes,  
sometimes called instance groups
or node pools, in your Kubernetes.

- For instance, you might have ten nodes with
high CPUs but low memory for applications that  
need a lot of computing power. Another group
might have a lot of memory but less CPU power.

- In cloud computing, it's common to
separate node pools into two types:  

  - on-demand nodes, which are available
    when you need them, and spot nodes,  

- Since application running in your Kubernetes is
not guaranteed to always run on the same node,  
you cannot use the local disk to save
data. If your app saves something on  
the local disk and then gets moved to another
node, that saved data won't be there anymore.

- That's why the local disk can only
be used for temporary cache storage.
To persist data, Kubernetes uses
something called Persistent Volumes.  

- While the CPU and memory resources of all nodes  
are pooled and managed by the Kubernetes
cluster, persistent file storage isn’t.
Instead, you can attach local or cloud
drives to the cluster as a Persistent  
Volume. You can think of it as plugging
an external hard drive into the cluster.

- Persistent Volumes provide
a file system that can be  
mounted to the cluster without being
associated with any particular node.
To run an application on the Kubernetes
cluster, you need to package it as a Linux  
container. Containerization allows you to create
self-contained Linux execution environments.

- Any application and all its dependencies
can be bundled up into a single image that  
can then be easily distributed. Anyone
can download the image and deploy it on  
their infrastructure with minimal setup required.

- Creating Docker images is usually part of
the CI/CD pipeline. You clone the repository,  
run some unit tests, and then build an image.
While it's possible to add multiple applications  
in a single container, you should aim to limit
yourself to one process per container if possible.

- It's better to have many small containers
than one large one. If the container  
has a tight focus updates are easier to
deploy, and issues are simpler to debug.

- Kubernetes doesn't run containers directly;
instead, it wraps one or more containers into  
a higher-level structure called a pod. Any
containers in the same pod will share the  
same resources and local network. Containers
can communicate with other containers in the  
same pod as they were on the same machine while
maintaining a degree of isolation from others.

- Pods used as the unit of replication in
Kubernetes. If your application needs to  
scale up, you simply increase the number of pods.
Kubernetes can be configured to automatically  
adjust the scale of your application based on
the load. You can use CPU, memory, or even custom  
metrics such as the number of requests to the
application. Typically, you would run multiple  
instances of the same application to prevent
downtime if something happens to a single node.

- As a container can have multiple processes, a
pod can contain multiple containers. However,  
since pods are scaled up and down as a unit,
all containers in a pod must be scaled together,  
regardless of their individual needs. This can
lead to wasted resources and a higher cost.

- To avoid this, pods should be kept as small
as possible, typically containing only a  
main process and its closely associated helper
containers, which we typically call sidecars.
Pods are the basic unit of
computation in Kubernetes,  
but they are not typically created
directly in the cluster. Instead,  
Kubernetes provides another level of
abstraction, such as a Deployment.

- The primary purpose of a Deployment
is to declare how many replicas of  
a pod should be running at any given time.
When a deployment is added to the cluster, it
automatically spins up the requested number of  
pods and then monitors them. If a pod fails,
the deployment will automatically recreate it.

- Using a deployment, you don't have to deal
with pods manually. You can just declare  
the desired state of the system, and it
will be managed for you automatically.

- By default, Kubernetes provides isolation
between pods and the outside world. If you want  
to communicate with a service running in a pod,
you have to open up a channel for communication.

- There are multiple ways to expose your service.
If you want to expose the application directly,
you can use the load balancer type. It will map  
one application per load balancer. In this case,
you can use almost any kind of protocol: TCP,  
UDP, gRPC, WebSockets, and others.
Another popular approach is the Ingress
controller. There are many different  
ingresses available for Kubernetes,
each with different capabilities.

- When using an ingress controller, you would share
a single load balancer among all your services  
and use subdomain or path to direct traffic to
a particular application within the cluster.
Ingresses only allow you to use
HTTP and HTTPS protocols. Also,  
they are more complicated to set up and
maintain over time than simple load balancers.
