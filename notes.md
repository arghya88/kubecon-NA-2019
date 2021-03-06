
Buildpacks
--------------------

Pivotal and Heroku presented intro to cloud native builpdack. Cloud native buildpack is implementation of CNB Spec(https://github.com/buildpack/spec). The platform interface is implemeneted by pack,kpack,heroku,tekton,google cloud run button. The docker image is built out of multiple layers and so if there is a CVE in OS Layer it can be rebuilt with a rebase.They demoed using Kpack to rebase images at scale in k8s automatically. Riff uses kpack as a building block to offer function as a service. Pack CLI uses docker in docker which can be avoided if Tekton is used.
There is a simillar but non overlapping project called cnab.io.


[Video](https://www.youtube.com/watch?v=SK6e_ZatOaw)

Pivotal and Heroku presented deep dive on cloud native buildpacks.

[Video](https://www.youtube.com/watch?v=j9Ak5YLrihU)

Service Mesh
--------------------

Updates from Kubecon
---------------------------------

Freddie Mac and Tetrate presented use cases and lessions learned from introducing istio in an enterprise having legacy technologies. They were able to use istio to migrate services from brownfield to greenfield (Kubernetes). Using istio they are able to abstract common features such as security, PKI, chaos engineering, micro segmentation, availability etc.K8s does not provide locality aware LB.They are using istio to achieve that. Using istio's additionl metrics such as request volume, response time, sttaus code for auto scaling.Mesh first containerize later approach.Traffic flows between services running on VMs and services running on k8s was enabled using istio.Challenges faced around:

Asymetric network connectivity - pods can talk to VMs but VMs can not talk to pods because it was not a flat network.
Certificate was from Venafi and application specific and was used by Weblogic and Websphere servers which hosted their legacy apps.
Incoming traffic and outgoing traffic to and from VMs through a sidecar typically does not play well with weblogic, websphere etc.
Service endpoints were scattered across multiple systems and with a centralized service registry its tough to use istio.
They talk about a use where they replaced a H/W LB with istio gateway.  

[Video](https://www.youtube.com/watch?v=Rako7zKXquU)

[presentation](https://static.sched.com/hosted_files/kccncna19/63/Tetrate%20-%20Freddie%20Mac%20-%20Istio%20Service%20Mesh.pdf)

Nordstorm presented their experience of using linkerd service mesh in an heterogenious environment consisting of Kubernetes on AWS, GKE and VMs on on prem and AWS EC2 and AWS Lambda. Low latency per requests and high scalability(memory per pod) of linkered led them to choose linkerd over other open source options such as istio. Forked linkerd to use AWS Certificate Manager Private Certificate Authority (ACM PCA). Using experimental feature of CNI Plugin capability of linkerd.

[Video](https://www.youtube.com/watch?v=sq8nsjuJqO4)

[Presentation](https://static.sched.com/hosted_files/kccncna19/d2/TACO-KubeCon19.pdf)

Department of Defence(DoD) has open sourced their DevSecOps stack. Baked in zero trust security using Sidecar Container Security Stack - istio, OPA and Knative

[Video](https://www.youtube.com/watch?v=YjZ4AZ7hRM0)


Pivotal presented envoy on windows. Pivotals first use case of envoy was container identity assurance using automatic cert rotation introduced in PCF 2.1.Pivotal has developed a port of envoy for windows after fixing a lot of problems and a lot to be fixed in future. Envoy community is engaged and microsoft has expressed interest in contributing to this port.

[Video](https://www.youtube.com/watch?v=FGBBeyZ-p1k)

[Presentation](https://static.sched.com/hosted_files/kccncna19/4a/Porting%20Envoy%20To%20Windows.pdf)

Airbnb had a homegrown software for service discovery called smartstack which uses Haproxy and Zookeeper. This system worked well for apps running on VMs but faced challenges when they tried to make it work for containers running on kubernetes. They faced scaling issue with Haproxy and Zookeeper config as number of backend services grew to thousands. Migrated to envoy and faced lots of challenges while migrating.

[Video](https://www.youtube.com/watch?v=XQjOhJtw1wg)

[Presentation](https://static.sched.com/hosted_files/kccncna19/f0/Airbnb%20Service%20Discovery%20-%20KubeCon%202019.pdf)

Intuit has 160+ k8s cluster across 4 different business unit which handles 1300+ deploys per day. Their application topology is such a way that micro services are deployed across multiple k8s cluster and they have mulpiple istio control planes across these clusters.They developed a management plane to manage istio control planes called admiral which is open source.

[Video](https://www.youtube.com/watch?v=EWyNbBn1vns)

[Presentation](https://static.sched.com/hosted_files/kccncna19/4e/admiral-v3-KUBECON2019.pdf)

Google presented how to use istio to migrate workloads from VMs progressively. Installed envoy on VM and configured it to istio control plane deployed on k8s. MESH_EXTERNAL and MESH_INTERNAL serviceentry is used to get client side metrics and full metrics respectively.

[Video](https://www.youtube.com/watch?v=0B8maYcjq_c)

[Presentation](https://static.sched.com/hosted_files/kccncna19/2d/Life%20Outside%20the%20Cluster%20%20-%20Kubecon%20NA%20%2719.pdf)

Lyft demonstrated service mesh using envoy in mobile. Envoy runs as library/engine.They developed a bridge layer between android/ios platfrom and envoy  native API. Envoy code is single threaded and does not use shared state.The benefits are same as the benfits of having server side envoy. Now they can see all metrics from mobile dendpoints.

[Video](https://www.youtube.com/watch?v=NYb_nVWkP-I)

[Presentation](https://static.sched.com/hosted_files/kccncna19/d1/EnvoyMobile_KubeCon2019.pdf)

Microsoft delivered a session on how to use service mesh benefits like canary deployment, A/B Testing etc. Flagger is a kubernetes operator for automating canary deploys using service mesh and metrics and supports linkerd, appmesh, nginx, gloo uses SMI.

[Video](https://www.youtube.com/watch?v=SMoaem3UBag)

[Presentation](https://static.sched.com/hosted_files/kccncna19/89/Supercharge%20your%20Microservices%20CICD%20with%20Service%20Mesh%20and%20Kubernetes.pdf)

Istio telemetry v2 was presented by Google.They are deprecating istio mixer and so there is no cache any more and stats extension runs in envoy directly. Wasm support is envoy is also exciting. Wasm is portable binary format. They are using wasm for stats extension and metadata exchange. Lot of these work is there in istio 1.4. In this talk they shared some CPU performance and latency between with mixer vs without mixer approach. Performance is better in new approach. They are also looking at integrating with open telemetry in future.


[Video](https://www.youtube.com/watch?v=I-3oHb6lqdU)

[Presentation](https://static.sched.com/hosted_files/kccncna19/d8/mesh-metrics-istio-v2%20%282%29.pptx)


Updates from NSMCon 
-------------------

Layer5 talked about adopting meshery with network service mesh. Meshery is the multi-service mesh management planeThey have a book called the enterprise path to service mesh architectures.

[Presentation](https://calcotestudios.com/talks/decks/slides-nsmcon-kubecon-na-2019-adopting-network-service-mesh-with-meshery.html)

There was a talk by cisco on combined usage of istio as application layer service mesh with L2/L3 layer network service mesh. Application service mesh solutions are great in single cloud environments but become unwieldy or unusable across multiple environments. They demoed istio service discovery working with NSM connected workloads where Envoy formed an application service mesh on top of the network mesh created by NSM.

[Presentation](https://drive.google.com/file/d/1gCstid5plU16ogcAlT83-EOvT25HNc_F/view?usp=sharing)

Updates from EnvoyCon 
---------------------

AWS has built service mesh called appmesh which uses envoy as data plane. It works with any kubernetes cluster, envoy running in EC2. Presentation includes high level archi of the control plane.

[Video](https://www.youtube.com/watch?v=eXv_4w2_Ohc)

[Presentation](https://static.sched.com/hosted_files/envoycon2019/19/AWSAppMesh_EnvoyCon.pdf)

Cilium/Isovalent presented namespacing concept of envoy.Similar to namespacing in the Linux kernel which serves as the foundation for containerization, namespaces for Envoy allow to isolate resources and thus share an Envoy instance among multiple application pods running on a single node without losing any of the isolation properties. This reduces the resource cost of running service mesh.

[Video](https://www.youtube.com/watch?v=08opgZkdYIw)

Lyft presented service mesh in kubernetes not easy. They built a control plane which munges service discovery data from legacy VMs which runs envoy and service discovery data from k8s endpoints APIs and send that info to all envoys using xDs streaming. The control plane runs in k8s as pods. They talk about issues that they faced primarily becuase fast scale up and down in k8s vs in legacy VMs. They had to fork k8s to ensure envoy is ready before the application starts.  Envoy federation work in progress to support multiple control plane.


[Video](https://www.youtube.com/watch?v=BTuDY2naDJI)

[Presentation](https://static.sched.com/hosted_files/envoycon2019/34/EnvoyCon%202019_%20Lita%20%26%20Tom.pdf)

Google presented next generation universal data plane API of envoy.Envoy v4 xDS API will be based on UDPA.Universal, cross-client/server data plane management API. CNCF governed working group; UDPA-WG

https://github.com/cncf/udpa

[Presentation](https://static.sched.com/hosted_files/envoycon2019/ac/EnvoyCon%20UDPA%202019.pdf)

Stripe presented their custom control plane built for envoy to support custom xDS services give control over routing, ramping Powers our blue/green deployments instead of DNS Tiered failovers (priority and locality), Manual incident remediation, Routing per customer, per request.

[Video](https://www.youtube.com/watch?v=QYCsd4Ptwzo)

[Presentation](https://static.sched.com/hosted_files/envoycon2019/4a/dylan-carney-envoycon.pdf)

Updates from ServiceMeshCon 
---------------------------
Hashicorp demoed migrated a service from a VM to kubernetes and performed traffic shifting without any downtime. They use consul service mesh using SPIFEE Id in an mTLs setup. Installed consul in both VM and kubernetes and connected to each other in federation mode. Hence service discovery between VM and kubernetes was achieved.

[Video](https://www.youtube.com/watch?v=v-ZYkGYi3jQ)

[Presentation](https://static.sched.com/hosted_files/servicemeshcon2019/af/ServiceMeshCon%20Connecting%20and%20Migrating%20Heterogeneous%20Applications%20with%20Consul%20Service%20Mesh.pdf)

Pinterest presented service mesh control plane in heterogenious environment. They have built a control plane using golang which serves envoy xDs v2 API and EDS. They also use zookeeper for storage and messaging.

[Video](https://www.youtube.com/watch?v=HkbSS7Af9n0)

[Presentation](https://static.sched.com/hosted_files/servicemeshcon2019/1c/Control%20Plane%20for%20a%20Mesh%20in%20a%20Heterogeneous%20Environment.pdf)

Serverless 
----------


