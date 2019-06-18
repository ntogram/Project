

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
 <br/>
 <p> Build MicroK8s with:</p>
 
 <code>git clone https://github.com/ubuntu/microk8s <br/></code>
    <code>   cd microk8s><br/> </code><br/>
     <code>  snapcraft cleanbuild</code>
 
 <p>Installing the snap</p>
 <code>snap install microk8s_latest_amd64.snap --classic --dangerous</code>
 
