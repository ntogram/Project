

 <head>
   <title> Cloud Project</title>
  </head>

<h1>An easy and automatic way for port configuration in microk8s</h1>
 <div>
<div> First we make  a short introduction in microk8s kubernetes<div>
  <br/><br/>
  <h2>Introduction</h2>
  <h3>Kubernates</h3>
 <p>Kubernetes is a portable, extensible open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.</p>
 <h3> Microk8s</h3>
 <p>
  MicroK8s is a CNCF certified upstream Kubernetes deployment that runs entirely in your workstation. Being a snap it runs all Kubernetes services natively (i.e. no virtual machines) while packing the entire set of libraries and binaries needed. Installation is limited by how fast you can download a couple of hundred megabytes and the removal of MicroK8s leaves nothing behind
 </p>
 </div>
 <div>
  <h2> Microk8s Installation</h2><br/><br/>
  <h3>Kubernetes in a snap that you can run locally.</h3>

<p>You can install MicroK8s with the latest stable upstream Kubernetes release with:</p>

<code>snap install microk8s --classic</code>
<br/><br/>
<h3>Building from source</h3><br/>
<p>To build the snap you need a working LXD installation. To install LXD on Ubuntu first remove any old packages:<p>

 <code> sudo apt-get purge lxc*<br/></code>
  <code> sudo apt-get purge lxd*</code><br/>
<p> Get the latest LXD and configure it with:
 </p>
 <code>
  sudo snap install lxd </code><br/>
 <code> sudo lxd init --auto
 </code>
 <br/><br/>
 <p> Build MicroK8s with:</p>
 
 <code>git clone https://github.com/ubuntu/microk8s <br/></code>
    <code>   cd microk8s><br/> </code>
     <code>  snapcraft cleanbuild</code>
 <br/><br/>
 <p>Installing the snap</p>
 <code>snap install microk8s_latest_amd64.snap --classic --dangerous</code>

 <br/>
 <br/>
 <div>
 The goal of this  project is to create   some small scripts to configure  with a user friendly way  the ports of the system where the following tools are executing:
 <ul>
  <li><a>kube-apiserver</a><br/>
   <p id='s1' style="display:none;">The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others. The API Server services REST operations and provides the frontend to the cluster’s shared state through which all other components interact.</p>
  </li>
  <li><a>kubelet</a>
 <div id='s2' style="display:none;" > <p>The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.</p>
   <p>There are three ways that a container manifest can be provided to the Kubelet.</p>
    <ul>
     <li>File: Path passed as a flag on the command line. Files under this path will be monitored periodically for updates. The         monitoring period is 20s by default and is configurable via a flag.</li>
      <li>HTTP endpoint: HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).</li>
     <li>HTTP server: The kubelet can also listen for HTTP and respond to a simple API (underspec’d currently) to submit a new manifest</li>
  </ul>
  <br/><p>
  The Pod Lifecycle Event Generator (PLEG) is a function of the kubelet that creates a list of the states for all containers and pods then compares it to the previous states of the containers and pods in a process called Relisting. This allows the PLEG to know which pods and containers need to be synced
  </p>
 
  <li><a>kube-controller-manager</a>
  <p id='s3'  style="display:none;" >The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation, a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state. Examples of controllers that ship with Kubernetes today are the replication controller, endpoints controller, namespace controller, and serviceaccounts controller.</p>
  
  </li>
  <li><a>kube-scheduler</a>
  <p id='s4'  style="display:none;" >
   The Kubernetes scheduler is a policy-rich, topology-aware, workload-specific function that significantly impacts availability, performance, and capacity. The scheduler needs to take into account individual and collective resource requirements, quality of service requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, deadlines, and so on. Workload-specific requirements will be exposed through the API as necessary.
 </p>
</li>
  <li><a>kube-proxy</a>
   <p id='s5'  style="display:none;">
    The Kubernetes network proxy runs on each node. This reflects services as defined in the Kubernetes API on each node and can do simple TCP, UDP, and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends. Service cluster IPs and ports are currently found through Docker-links-compatible environment variables specifying ports opened by the service proxy. There is an optional addon that provides cluster DNS for these cluster IPs. The user must create a service with the apiserver API to configure the proxy.
</p>


</li>
 </div>
 
