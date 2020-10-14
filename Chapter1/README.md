<div align="center" id="top"> 
  <img src="https://kubernetes.io/images/nav_logo2.svg" style="width: 50vw; min-width: 200px;" alt="K8s" />

  &#xa0;

  <!-- <a href="https://k8s.netlify.app">Demo</a> -->
</div>

<h1 align="center">K8s Chapter 1</h1>

<!-- <p align="center">
  <img alt="Github top language" src="https://img.shields.io/github/languages/top/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8">

  <img alt="Github language count" src="https://img.shields.io/github/languages/count/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8">

  <img alt="Repository size" src="https://img.shields.io/github/repo-size/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8">

  <img alt="License" src="https://img.shields.io/github/license/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8">

  <!-- <img alt="Github issues" src="https://img.shields.io/github/issues/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8" /> -->

  <!-- <img alt="Github forks" src="https://img.shields.io/github/forks/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8" /> -->

  <!-- <img alt="Github stars" src="https://img.shields.io/github/stars/{{YOUR_GITHUB_USERNAME}}/k8s?color=56BEB8" /> -->

<!-- Status -->

<!-- <h4 align="center"> 
	
</h4> 

<hr> -->

<p align="center">
  <a href="#dart-Introduction">Introduction</a> &#xa0; | &#xa0; 
  <a href="#sparkles-Command">Command</a> &#xa0; | &#xa0;
  <a href="#rocket-Output">Output</a> &#xa0; | &#xa0;
  <a href="#memo-license">License</a> &#xa0; | &#xa0;
  <a href="https://github.com/codeandqa" target="_blank">Author</a>
</p>

<br>

## :dart: Introduction ##

The basic building block of K8s cluser it a Pod where we run our containers. In this chapter we will start Pod and see if that is running.

## :sparkles: Command ##

```bash
# Firt check kubectl is running
$ kubectl get all

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32d

#create a Pod
$ kubectl create -f Pod.yml
pod/nginx-app created

$ kubectl get all
NAME            READY   STATUS    RESTARTS   AGE
pod/nginx-app   1/1     Running   0          4m17s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32d

```

## :rocket: Output ##

To get the detail info of the above pod run ```describe``` command:

```bash
# $ kubectl describe Pod [PodName]
$ Kubectl describe pod nginx-app
Name:         nginx-app
Namespace:    default
Priority:     0
Node:         docker-desktop/192.168.65.3
Start Time:   Wed, 14 Oct 2020 01:09:57 -0400
Labels:       name=nginx-app
Annotations:  <none>
Status:       Running
IP:           10.1.0.63
IPs:
  IP:  10.1.0.63
Containers:
  nginx-app:
    Container ID:   docker://cab09b1b56f1eb17a5387691cc10fb4712544ffefb424334732185caf549b8d4
    Image:          adityashahi/myapp
    Image ID:       docker-pullable://adityashahi/myapp@sha256:57426447bd7f1583d404d79f9da153b02782e535ba0a057de8067e2dd90ebba3
    Port:           30080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Wed, 14 Oct 2020 01:10:00 -0400
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     500m
      memory:  128Mi
    Requests:
      cpu:        500m
      memory:     128Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8svlg (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-8svlg:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-8svlg
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From                     Message
  ----    ------     ----       ----                     -------
  Normal  Scheduled  <unknown>  default-scheduler        Successfully assigned default/nginx-app to docker-desktop
  Normal  Pulling    5m26s      kubelet, docker-desktop  Pulling image "adityashahi/myapp"
  Normal  Pulled     5m25s      kubelet, docker-desktop  Successfully pulled image "adityashahi/myapp"
  Normal  Created    5m24s      kubelet, docker-desktop  Created container nginx-app
  Normal  Started    5m24s      kubelet, docker-desktop  Started container nginx-app
```
IP of the pod is `10.1.0.63` and I would assume that I should see the webserver on that given ip address:
lets try to curl:

```bash
$ curl http://10.1.0.63

curl : Unable to connect to the remote server
At line:1 char:1
+ curl http://10.1.0.63
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
```
you can try to connect via brower and you will not see anything while nginx is running. This is happening because your pod is an independent network and your browser is not a part of that network. To reach out to any website you never hit the server directly but that network as some sort of load balancer and that load balncer drive the traffic to the right server. In Kubernetes, we have Service that helps us to reach to the server. Pods connect to the world via Service. We need to create a service and make sure that it attached to the pod we are trying to reach out. 

Lets delete the existing pod and move to the next section where we will create new pod and new service and try to reach out to the server.

```bash
#Delete the pod.
$ kubectl delete pod nginx-app
pod "nginx-app" deleted
```

## :memo: License ##

This project is under license from MIT. For more details, see the [LICENSE](LICENSE.md) file.


Made with :heart: by <a href="https://github.com/anwisys" target="_blank">Aditya Kumar</a>

&#xa0;

<a href="#top">Back to top</a>
