# Kubernetes

Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit.

A Kubernetes cluster consists of two types of resources:

- The **Master** coordinates the cluster
- **Nodes** are the workers that run applications

A Kubernetes cluster can be deployed on either physical or virtual machines. A Kubernetes cluster that handles production traffic should have a minimum of three nodes.

Minikube is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node. Minikube is available for Linux, macOS, and Windows systems.

```
minikube version
minikube start
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl run APP --image=IMAGE:TAG --port=PORT
kubectl get deployments
kubectl proxy
```

Kubernetes **Deployment** configuration instructs Kubernetes how to create and update instances of your application. Once the application instances are created, a Kubernetes Deployment Controller continuously monitors those instances. If the Node hosting an instance goes down or is deleted, the Deployment controller replaces the instance with an instance on another Node in the cluster. **This provides a self-healing mechanism to address machine failure or maintenance.**

When you create a Deployment, you'll need to specify the container image for your application and the number of replicas that you want to run. You can change that information later by updating your Deployment.

A **Pod** is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker or rkt), and some shared resources for those containers. Those resources include:

- Shared storage, as Volumes
- Networking, as a unique cluster IP address
- Information about how to run each container, such as the container image version or specific ports to use

The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.

Each Pod is tied to the Node where it is scheduled, and remains there until termination (according to restart policy) or deletion. In case of a Node failure, identical Pods are scheduled on other available Nodes in the cluster.

A Pod is a group of one or more application containers (such as Docker or rkt) and includes shared storage (volumes), IP address and information about how to run them.

Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk.

A Pod always runs on a Node. A Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster.

A **Service** routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application.

Each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service. A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them.

Services match a set of Pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes.

**Scaling** is accomplished by changing the number of replicas in a Deployment. Services have an integrated load-balancer that will distribute network traffic to all Pods of an exposed Deployment. Services will monitor continuously the running Pods using endpoints, to ensure the traffic is sent only to available Pods.

Once you have multiple instances of an Application running, you would be able to do Rolling updates without downtime. Rolling updates allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones.

A StatefulSet manages Pods that are based on an identical container spec. For a StatefulSet with N replicas, when Pods are being deployed, they are created sequentially. Pods in a StatefulSet have a unique ordinal index and a stable network identity.

Deleting and/or scaling a StatefulSet down will not delete the volumes associated with the StatefulSet. StatefulSets currently require a Headless Service to be responsible for the network identity of the Pods.

Copy /tmp/foo local file to /tmp/bar in a remote pod in namespace

```
kubectl cp /tmp/foo <some-namespace>/<some-pod>:/tmp/bar
```

Copy /tmp/foo from a remote pod to /tmp/bar locally

```
kubectl cp <some-namespace>/<some-pod>:/tmp/foo /tmp/bar
```

Restart pods

```
kubectl scale --replicas=0 deployment DEPLOYMENT
kubectl scale --replicas=1 deployment DEPLOYMENT
```
kubectl -n prod describe pod POD
kubectl delete deploy DEPLOYMENT
