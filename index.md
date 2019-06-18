

 <head>
   <title> Cloud Project</title>
 <style> 
#s1,#s2 ,#s3,#s4,#s5{
 display:none
}
</style>
 
 
 
 
  </head>

<h1>An easy and automatic way for port configuration in microk8s</h1>
 <div><br/>
<div> First we make  a short introduction in microk8s kubernetes<div>
  <br/><br/>
  <h2>Introduction</h2><br/>
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
 
 <h2> Project Goal</h2><br/>
 The goal of this  project is to create   some small scripts to configure  with a user friendly way  the ports of the system where the following tools are executing.Click on  tools for more elements
 
 <h2> Summary for the tools</h2>
 
 <ul>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s1").style.display = "block";'>kube-apiserver</a><br/>
   <p id='s1'>The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others. The API Server services REST operations and provides the frontend to the cluster’s shared state through which all other components interact.</p>
  </li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s2").style.display = "block";' >kubelet</a>
 <div id='s2' > <p>The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.</p>
   <p>There are three ways that a container manifest can be provided to the Kubelet.</p>
    <ul>
     <li>File: Path passed as a flag on the command line. Files under this path will be monitored periodically for updates. The         monitoring period is 20s by default and is configurable via a flag.</li>
      <li>HTTP endpoint: HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).</li>
     <li>HTTP server: The kubelet can also listen for HTTP and respond to a simple API (underspec’d currently) to submit a new manifest</li>
  </ul>
  <br/><p>
  The Pod Lifecycle Event Generator (PLEG) is a function of the kubelet that creates a list of the states for all containers and pods then compares it to the previous states of the containers and pods in a process called Relisting. This allows the PLEG to know which pods and containers need to be synced
  </p>
 
  <li><a href="javascript:void(0);" onclick='document.getElementById("s3").style.display = "block";'>kube-controller-manager</a>
  <p id='s3'>The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation, a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state. Examples of controllers that ship with Kubernetes today are the replication controller, endpoints controller, namespace controller, and serviceaccounts controller.</p>
  
  </li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s4").style.display = "block";' >kube-scheduler</a>
  <p id='s4'>
   The Kubernetes scheduler is a policy-rich, topology-aware, workload-specific function that significantly impacts availability, performance, and capacity. The scheduler needs to take into account individual and collective resource requirements, quality of service requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, deadlines, and so on. Workload-specific requirements will be exposed through the API as necessary.
 </p>
</li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s5").style.display = "block";'>kube-proxy</a>
   <p id='s5'  style="display:none;">
    The Kubernetes network proxy runs on each node. This reflects services as defined in the Kubernetes API on each node and can do simple TCP, UDP, and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends. Service cluster IPs and ports are currently found through Docker-links-compatible environment variables specifying ports opened by the service proxy. There is an optional addon that provides cluster DNS for these cluster IPs. The user must create a service with the apiserver API to configure the proxy.
</p>


</li>
 </div>
 <h2> Port Configuration</h2><br/>
 
 <h3>Description</h3><br/>
 <p> The  process for port configuration is almost the same for all tools.</p>
 <ol>
  <li>Update the corresponding  args  file with new port. Args files for these services are found in path: /var/snap/microk8s/current/args/. The option flag argument for port has a different name</li>
  <li>Inform other services for new port</li>
  <li> Create new, updated copies of our kubeconfig for kubelet and kubectl to use</li>
 <li>Inform kubelet about the new kubeconfig</li>
 <li>Disable and enable the microk8s snap to restart all services</li>
 <li>Check if configuration process is  executed correctly by running microk8s inspect and the necessary command i terminal for checking if  all services are running  in correct new ports.</li>
</ol>  
 
 <br/>
 
 
 
 
 
 <h3> Table for the ports  and flags that must be defined for ports reconfiguraton</h3><br/>
 <table>
  <thead>
    <tr>
      <th>Port</th>
      <th>Service</th>
      <th>Description</th>
     <th>Option Flag for port configuration</th>
    </tr>
 </thead>
  <tbody>
    <tr>
      <td>8080</td>
      <td>API server</td>
      <td>Port for insecure communication to the API server</td>
     <td>insecure-port</td>
    </tr>
    <tr>
      <td>10248</td>
      <td>kubelet</td>
      <td>Localhost healthz endpoint.</td>
     <td>healthz-port</td>
    </tr>
    <tr>
      <td>10249</td>
      <td>kube-proxy</td>
      <td>Port for the metrics server to serve on.</td>
     <td>metrics-port</td>
    </tr>
    <tr>
      <td>10251</td>
      <td>kube-scheduler</td>
      <td>Port on which to serve HTTP insecurely.</td>
      <td>port</td>
    </tr>
    <tr>
      <td>10252</td>
      <td>kube-controller</td>
      <td>Port on which to serve HTTP insecurely.</td>
     <td>port</td>
    </tr>
    <tr>
      <td>10256</td>
      <td>kube-proxy</td>
      <td>Port to bind the health check server.</td>
      <td>healthz-port</td> 
    </tr>
  </tbody>
</table>
 <br/>
 <div>
 <h3> Prerequirements</h3>
<br/>
 <p> This program has been  checked in the latest linux ubuntu version</p>
 <p> It would be great  if you have already installed the following packages</p>
 <ul>
  <li>net-tools:it is used for checking which ports of the system are available</li>
  <li>jq:It is used for reading a json file.</li>
 </ul>
 <p> if you don't have installed  these packages don't worry you can install them by executing in terminal the following  commands:</p>
 <ul>
  <li><code>sudo apt install net-tools</code></li>
  <li><code>sudo apt install jq</code></li>
 </ul> 
 <p> Hint:these package  will be  installed the first time the main program for microk8s_port_conf.sh  port configuration  will be executed</p>
 <br/>
 <h3> Instructions for using   scripts for  port configuration</h3>
 <br/>
 <ol>
  <li> Download code from <a href="

 <head>
   <title> Cloud Project</title>
 <style> 
#s1,#s2 ,#s3,#s4,#s5{
 display:none
}
</style>
 
 
 
 
  </head>

<h1>An easy and automatic way for port configuration in microk8s</h1>
 <div><br/>
<div> First we make  a short introduction in microk8s kubernetes<div>
  <br/><br/>
  <h2>Introduction</h2><br/>
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
 
 <h2> Project Goal</h2><br/>
 The goal of this  project is to create   some small scripts to configure  with a user friendly way  the ports of the system where the following tools are executing.Click on  tools for more elements
 
 <h2> Summary for the tools</h2>
 
 <ul>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s1").style.display = "block";'>kube-apiserver</a><br/>
   <p id='s1'>The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others. The API Server services REST operations and provides the frontend to the cluster’s shared state through which all other components interact.</p>
  </li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s2").style.display = "block";' >kubelet</a>
 <div id='s2' > <p>The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.</p>
   <p>There are three ways that a container manifest can be provided to the Kubelet.</p>
    <ul>
     <li>File: Path passed as a flag on the command line. Files under this path will be monitored periodically for updates. The         monitoring period is 20s by default and is configurable via a flag.</li>
      <li>HTTP endpoint: HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).</li>
     <li>HTTP server: The kubelet can also listen for HTTP and respond to a simple API (underspec’d currently) to submit a new manifest</li>
  </ul>
  <br/><p>
  The Pod Lifecycle Event Generator (PLEG) is a function of the kubelet that creates a list of the states for all containers and pods then compares it to the previous states of the containers and pods in a process called Relisting. This allows the PLEG to know which pods and containers need to be synced
  </p>
 
  <li><a href="javascript:void(0);" onclick='document.getElementById("s3").style.display = "block";'>kube-controller-manager</a>
  <p id='s3'>The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation, a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state. Examples of controllers that ship with Kubernetes today are the replication controller, endpoints controller, namespace controller, and serviceaccounts controller.</p>
  
  </li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s4").style.display = "block";' >kube-scheduler</a>
  <p id='s4'>
   The Kubernetes scheduler is a policy-rich, topology-aware, workload-specific function that significantly impacts availability, performance, and capacity. The scheduler needs to take into account individual and collective resource requirements, quality of service requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, deadlines, and so on. Workload-specific requirements will be exposed through the API as necessary.
 </p>
</li>
  <li><a href="javascript:void(0);" onclick='document.getElementById("s5").style.display = "block";'>kube-proxy</a>
   <p id='s5'  style="display:none;">
    The Kubernetes network proxy runs on each node. This reflects services as defined in the Kubernetes API on each node and can do simple TCP, UDP, and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends. Service cluster IPs and ports are currently found through Docker-links-compatible environment variables specifying ports opened by the service proxy. There is an optional addon that provides cluster DNS for these cluster IPs. The user must create a service with the apiserver API to configure the proxy.
</p>


</li>
 </div>
 <h2> Port Configuration</h2><br/>
 
 <h3>Description</h3><br/>
 <p> The  process for port configuration is almost the same for all tools.</p>
 <ol>
  <li>Update the corresponding  args  file with new port. Args files for these services are found in path: /var/snap/microk8s/current/args/. The option flag argument for port has a different name</li>
  <li>Inform other services for new port</li>
  <li> Create new, updated copies of our kubeconfig for kubelet and kubectl to use</li>
 <li>Inform kubelet about the new kubeconfig</li>
 <li>Disable and enable the microk8s snap to restart all services</li>
 <li>Check if configuration process is  executed correctly by running microk8s inspect and the necessary command i terminal for checking if  all services are running  in correct new ports.</li>
</ol>  
 
 <br/>
 
 
 
 
 
 <h3> Table for the ports  and flags that must be defined for ports reconfiguraton</h3><br/>
 <table>
  <thead>
    <tr>
      <th>Port</th>
      <th>Service</th>
      <th>Description</th>
     <th>Option Flag for port configuration</th>
    </tr>
 </thead>
  <tbody>
    <tr>
      <td>8080</td>
      <td>API server</td>
      <td>Port for insecure communication to the API server</td>
     <td>insecure-port</td>
    </tr>
    <tr>
      <td>10248</td>
      <td>kubelet</td>
      <td>Localhost healthz endpoint.</td>
     <td>healthz-port</td>
    </tr>
    <tr>
      <td>10249</td>
      <td>kube-proxy</td>
      <td>Port for the metrics server to serve on.</td>
     <td>metrics-port</td>
    </tr>
    <tr>
      <td>10251</td>
      <td>kube-scheduler</td>
      <td>Port on which to serve HTTP insecurely.</td>
      <td>port</td>
    </tr>
    <tr>
      <td>10252</td>
      <td>kube-controller</td>
      <td>Port on which to serve HTTP insecurely.</td>
     <td>port</td>
    </tr>
    <tr>
      <td>10256</td>
      <td>kube-proxy</td>
      <td>Port to bind the health check server.</td>
      <td>healthz-port</td> 
    </tr>
  </tbody>
</table>
 <br/>
 <div>
 <h3> Prerequirements</h3>
<br/>
 <p> This program has been  checked in the latest linux ubuntu version</p>
 <p> It would be great  if you have already installed the following packages</p>
 <ul>
  <li>net-tools:it is used for checking which ports of the system are available</li>
  <li>jq:It is used for reading a json file.</li>
 </ul>
 <p> if you don't have installed  these packages don't worry you can install them by executing in terminal the following  commands:</p>
 <ul>
  <li><code>sudo apt install net-tools</code></li>
  <li><code>sudo apt install jq</code></li>
 </ul> 
 <p> Hint:these package  will be  installed the first time the main program for microk8s_port_conf.sh  port configuration  will be executed</p>
 <br/>
 <h3> Instructions for using   scripts for  port configuration</h3>
 <br/>
 <ol>
  <li> Download code from <a href="https://drive.google.com/file/d/1zlwRBGEluaq0WdT6w9DytS06D_8pTi40/" target="_blank">code</a></li>
  <li> Go to the folder where the downloaded zip file was stored and unzip it</li>
  <li> Open the terminal and execute the following command:<br/>
   <code>cd &lt; path of the unziped folder &gt;</code> </li>
    <li> For every .sh file in this folder execute <br/>
     <code> chmod +x &lt; filename.sh  &gt; </code><div> or more specifically</div><br/>
      <code>chmod +x change_kube_apiserverport.sh </code><br/>
       <code>chmod +x change_kubeletport.sh </code><br/>
        <code>chmod +x change_kube_schedulerport.sh </code><br/>
       <code>chmod +x change_kube_controllerport.sh </code><br/>
       <code>chmod +x change_kube_proxyports.sh </code><br/>
     <code>chmod +x microk8s_port_conf.sh </code></li></ol>
     <br/>
     <p> We are ready to use these scripts</p>
 <h4> How to execute these scripts?</h4>
  <br/> <p> Actually the users  should execute only main script microk8s_port_conf.sh. The other scripts  make port configurations for services and are executed via microk8s_port_conf.sh . User can  execute the other scripts but he must  read and understand the whole  structure and process of this project which is much complicated.</p>
<p> For executing microk8s_port_conf.sh you should give some arguments   before executing the script. The  first argument is mode  argument . User has   4 options:
 <ul>
  <li> help: give an complete manual for using  this script. Help Mode  for port configuration script is given by  executing  in command  in the folder of script:<br/><br/>
   <code> ./microk8s_port_conf.sh help</code></li><br/>
  <li>auto:Reset services port to default values. If a or some services use the default port , program inform the use that this  service has already used this port.Auto Mode for port configuration script is given by  executing  in command  in the folder of script:<br/>
 <code> ./microk8s_port_conf.sh auto</code></li><br/>
  <li> json:Before executing the script in this mode, you should make  a json file that has  a  json object. Every element in this json should has  the followin format &lt kubernetes_sevice_name &gt : &lt port number &gt. An example of  json for this script is the following:<br/><br/>
   <code>
  {
"kube_apiserver":8082,
"kubelet":10283,
"kube_scheduler":10278,
"kube_controller_manager":10291,
"kube_proxy_metrics":10281,
"kube_proxy_healthz":10280
}
</code>  <br/>
   
  <br/><p>Another json example exist along with code.In json  file,Services names can  be given in any order and some service can be omitted- this is for services that should not  change port<br/>
Json Mode for port configuration script is given by  executing  in command  in the folder of script:<br/><br/></p>
 <code> ./microk8s_port_conf.sh json &lt; filename &gt; .json</code> <br/><br/>
  <p>If user gives as second argument  a  file tha is not json or  a json file that does not exist, program return a coresponding error message.</p>
  <p> Program use jq   to get port values from  json file</p>
  </li>
  <li>parameters:users can give ports directly to the program as arguments when parameters mode is used but in contrast to json here services ports are not given  in any order. User should give  the ports with the following order:
 <ol>
  <li>kube-apiserver port</li>
   <li>kubeletvport</li>
   <li>kube-scheduler port</li>
   <li>kube-controller_manager port</li>
   <li> proxy port: for metrics></li>
   <li> proxy port:for checking health</li>
 </ol><br/>
<p> The last port services can be omitted if  user don't want change their ports but  if users dont' want to change port for a service in the middle, they must give as argument  fr the port of  this service  the term  "none"</p><br/>
 <p>Some examples:</p>
 <br/>
 <code> ./microk8s_port_conf.sh parameters 8081 8082 8083 8084 8085 8086 <br/>
  ./microk8s_port_conf.sh parameters 8081 8082 8083 8084 <br/>
  ./microk8s_port_conf.sh parameters none none 8083 8084 8085 8086</code>
 </li>
 </ul>
<br/><p> Finally  in the microk8s_port_conf.sh, we check the  values  that users give as port numbers. Firstly   we check if values are not integers. For this case program print in terminal error message  that inform user that some of the parameters are not valid. We also check if the  port given is used. Here there are two cases. The first case as we have already mentioned above is that this service  use the given port and  program print that this service has already used this port. In the second case, the given port is used by another service and the program inform the users that this port is unavailable.
 Program print corresponding messages for the cases that users don't give a port for one or some service.</p>" target="_blank">code</a></li>
  <li> Go to the folder where the downloaded zip file was stored and unzip it</li>
  <li> Open the terminal and execute the following command:<br/>
   <code>cd &lt; path of the unziped folder &gt;</code> </li>
    <li> For every .sh file in this folder execute <br/>
     <code> chmod +x &lt; filename.sh  &gt; </code><div> or more specifically</div><br/>
      <code>chmod +x change_kube_apiserverport.sh </code><br/>
       <code>chmod +x change_kubeletport.sh </code><br/>
        <code>chmod +x change_kube_schedulerport.sh </code><br/>
       <code>chmod +x change_kube_controllerport.sh </code><br/>
       <code>chmod +x change_kube_proxyports.sh </code><br/>
     <code>chmod +x microk8s_port_conf.sh </code></li></ol>
     <br/>
     <p> We are ready to use these scripts</p>
 <h4> How to execute these scripts?</h4>
  <br/> <p> Actually the users  should execute only main script microk8s_port_conf.sh. The other scripts  make port configurations for services and are executed via microk8s_port_conf.sh . User can  execute the other scripts but he must  read and understand the whole  structure and process of this project which is much complicated.</p>
<p> For executing microk8s_port_conf.sh you should give some arguments   before executing the script. The  first argument is mode  argument . User has   4 options:
 <ul>
  <li> help: give an complete manual for using  this script. Help Mode  for port configuration script is given by  executing  in command  in the folder of script:<br/><br/>
   <code> ./microk8s_port_conf.sh help</code></li><br/>
  <li>auto:Reset services port to default values. If a or some services use the default port , program inform the use that this  service has already used this port.Auto Mode for port configuration script is given by  executing  in command  in the folder of script:<br/>
 <code> ./microk8s_port_conf.sh auto</code></li><br/>
  <li> json:Before executing the script in this mode, you should make  a json file that has  a  json object. Every element in this json should has  the followin format &lt kubernetes_sevice_name &gt : &lt port number &gt. An example of  json for this script is the following:<br/><br/>
   <code>
  {
"kube_apiserver":8082,
"kubelet":10283,
"kube_scheduler":10278,
"kube_controller_manager":10291,
"kube_proxy_metrics":10281,
"kube_proxy_healthz":10280
}
</code>  <br/>
   
  <br/><p>Another json example exist along with code.In json  file,Services names can  be given in any order and some service can be omitted- this is for services that should not  change port<br/>
Json Mode for port configuration script is given by  executing  in command  in the folder of script:<br/><br/></p>
 <code> ./microk8s_port_conf.sh json &lt; filename &gt; .json</code> <br/><br/>
  <p>If user gives as second argument  a  file tha is not json or  a json file that does not exist, program return a coresponding error message.</p>
  <p> Program use jq   to get port values from  json file</p>
  </li>
  <li>parameters:users can give ports directly to the program as arguments when parameters mode is used but in contrast to json here services ports are not given  in any order. User should give  the ports with the following order:
 <ol>
  <li>kube-apiserver port</li>
   <li>kubeletvport</li>
   <li>kube-scheduler port</li>
   <li>kube-controller_manager port</li>
   <li> proxy port: for metrics></li>
   <li> proxy port:for checking health</li>
 </ol><br/>
<p> The last port services can be omitted if  user don't want change their ports but  if users dont' want to change port for a service in the middle, they must give as argument  fr the port of  this service  the term  "none"</p><br/>
 <p>Some examples:</p>
 <br/>
 <code> ./microk8s_port_conf.sh parameters 8081 8082 8083 8084 8085 8086 <br/>
  ./microk8s_port_conf.sh parameters 8081 8082 8083 8084 <br/>
  ./microk8s_port_conf.sh parameters none none 8083 8084 8085 8086</code>
 </li>
 </ul>
<br/><p> Finally  in the microk8s_port_conf.sh, we check the  values  that users give as port numbers. Firstly   we check if values are not integers. For this case program print in terminal error message  that inform user that some of the parameters are not valid. We also check if the  port given is used. Here there are two cases. The first case as we have already mentioned above is that this service  use the given port and  program print that this service has already used this port. In the second case, the given port is used by another service and the program inform the users that this port is unavailable.
 Program print corresponding messages for the cases that users don't give a port for one or some service.</p>
