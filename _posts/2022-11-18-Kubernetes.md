---
title: "Kubernetes"
layout: post
---

Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. It provides a unified way of deploying, scaling, and operating applications across different infrastructure environments, such as on-premises data centers, public clouds, etc.

<img class="post-image" src="{{ '/assets/media/kubernetes-1.jpg' }}" alt="avatar" onerror="this.style.display='none'">

<h2>Proposed Solution</h2>
I propose you k8s HA cluster using Rancher on the VMware cluster Platform.
ESXi is a popular virtualization platform that allows you to run multiple virtual machines on a single physical host. By creating an ESXi cluster with multiple physical hosts, you can pool resources and provide high availability for your virtual machines. This ensures that your applications remain available even if one or more hosts fail.
Rancher is a popular choice for deploying and managing Kubernetes clusters on VMware ESXi clusters as it provides a user-friendly interface for k8s cluster management, including a centralized dashboard for monitoring cluster health and performance. It also supports advanced Kubernetes features like automatic scaling, service discovery, and load balancing, making it a good choice for high-availability deployments.

With Rancher, you can easily deploy a Kubernetes cluster with multiple control plane nodes and worker nodes, ensuring that your cluster remains available even in the event of node failures. Additionally, Rancher also provides robust backup and disaster recovery features, ensuring that your data is protected and can be easily restored in the event of a failure. 
Furthermore, you can use the VMware ESXi cluster to create both traditional virtual machines as well as Kubernetes virtual machines. The VMware ESXi cluster provides a virtualized infrastructure that allows you to create, manage, and run multiple virtual machines on the same physical hardware. This can include running Kubernetes virtual machines alongside traditional virtual machines, as long as the underlying hardware and resources are for workloads.
Following is the proposed Hardware that will be used for the VMware ESXI cluster for further k8s deployment.

<ul>
  <li>
    <span>a) Suggested Hardware for VMware cluster</span>

<img class="post-image" src="{{ '/assets/media/kubernetes-3.png' }}" alt="avatar" onerror="this.style.display='none'">

We can also use your existing physical hosts as well for k8s cluster.
For data storage, it's recommended to use multiple disks or disk arrays in each host to provide redundancy and performance. Using solid-state drives (SSDs) can provide better performance for workloads with high I/O demands, while using traditional hard disk drives.
Note: This is a Suggested hardware it all depend upon the usage of the resources. We can go with low specs as well as per actual requirements.
  </li>
<li>
  <span>b) VMs specification for K8s cluster</span>

<img class="post-image" src="{{ '/assets/media/kubernetes-4.png' }}" alt="avatar" onerror="this.style.display='none'">

<i>Note: VM specification depends on usage we can adjust accordingly.</i>  
</li>
<li>  
  <span>c) Proposed Architecture</span> 
  <img class="post-image" src="{{ '/assets/media/kubernetes-2.png' }}" alt="avatar" onerror="this.style.display='none'">
</li>  
<li>
  <span>d) Design Explain</span>
  This design consists of a VMware ESXi cluster with three physical hosts and a management network of 10.101.10.0/24. The cluster has a VM named Rancher deployed on it with a management NIC of 10.101.10.0/24. Using Rancher, a Kubernetes HA cluster is deployed on the ESXi hosts consisting of three master nodes with 2xNIC, one for the provider network of 192.168.0/24 and the other for the management network of 10.101.10.0/24. master node is responsible for managing the overall state of the cluster. It schedules and manages the deployment of containers, monitors the status of nodes and containers, and makes decisions about scaling the cluster. The master node consists of several components, including the kube-apiserver, kube-scheduler, and kube-controller-manager.
The Kubernetes cluster also has two worker nodes and an optional worker node, each with 2xNIC for provider and management networks. Worker nodes are the actual compute nodes that run the containerized applications. They receive tasks from the master node and execute them by creating and managing the containers. Each worker node runs a kubelet process that communicates with the master node and a container runtime, such as Docker or containerd, to manage the containers. The worker nodes also run other Kubernetes components, such as kube-proxy, to manage networking between containers.
</li>
<li>
  <span>e) Installation Steps</span>
  <ol>
      <li>Install VMware ESXi on each of the three physical hosts.</li>
      <li>Create a cluster in VMware vSphere Client by selecting all three ESXi hosts and creating a new cluster. This will allow you to manage the hosts together as a single entity.</li>
      <li>Create two virtual switches in VMware vSphere Client: one for the Management Network and another for the Provider Network. Assign the Management Network to the physical NIC that is connected to the Management Network on each host and assign the Provider Network to the physical NIC that is connected to the Provider Network on each host.</li>
      <li>Create a new VM on the ESXi cluster using the Rancher ISO image.</li>
      <li>Install Rancher on the newly created VM and configure it for your environment. This will allow you to manage your Kubernetes cluster using the Rancher UI.</li>
      <li>Deploy a Kubernetes cluster using Rancher by selecting the option to "Launch Kubernetes".</li>
      <li>Create three Master nodes for the Kubernetes cluster with two NICs each: one for the Provider Network (192.168.0/24) and one for the Management Network (10.101.10.0/24).</li>
      <li>Each Master node will be installed on physical server-1, Physical server-2, and Physical server-3.</li>
      <li>Create two Worker nodes for the Kubernetes cluster with two NICs each: one for the Provider Network (192.168.0/24) and one for the Management Network (10.101.10.0/24).</li>
      <li>Same as K8s master Node, Worker nodes will also install on physical server-1, Physical server-2 and Physical server-3 accordingly</li>
      <li>Optionally, create an additional Worker node with two NICs: one for the Provider Network (192.168.0/24) and one for the Management Network (10.101.10.0/24).</li>
      <li>Install a HAProxy load balancer for the Kubernetes cluster with two NICs: one for the Provider Network (192.168.0/24) and one for the Management Network (10.101.10.0/24).</li>
  </ol>

  With these steps, you should have a fully functional Kubernetes cluster running on your VMware ESXi cluster, managed by Rancher, and accessible through the HAProxy load balancer.
</li>
</ul>
