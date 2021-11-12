# Kubernetes Labels and Annotations

## Introduction
The effectiveness of the Kubernetes API comes from how it manages the Kubernetes resources via metadata: labels and annotations. Metadata is essential for grouping resources, redirecting requests and managing deployments. In addition, is is also used to troubleshoot Kubernetes applications.
## Use Case
Labels can be used to help manage release management of your applications/deployments into your clusters, as an example.

- Labels - used by Kubernetes to make decisions and identify resources, such as which pods to load balance incoming traffic to. 
- Annotations - Used to store non-arbitary data for resources, such as contact details of who manages the resource. This data will not be used by Kubernetes.
## Research
Blog - [Best Practices Guide for Kubernetes Labels and Annotations](https://komodor.com/blog/best-practices-guide-for-kubernetes-labels-and-annotations/)

Kubernetes Docs - [Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)  - [Annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) - [Common Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/) - [Well Know Labels, Annotations and Taints](https://kubernetes.io/docs/reference/labels-annotations-taints/)

If you want to take this further, I previously [wrote a blog post](https://veducate.co.uk/tanzu-mission-control-custom-policy-kasten/) looking at how we can use Kubernetes Admission Control [Open Policy Agent (OPA)](https://www.openpolicyagent.org/docs/latest/) to ensure Labels are defined on resources to ensure our backup software protects the data. 
## Try yourself
Get the labels for a node.

1. This will output all of the information for this node, however at the top of this, you will see annotations and labels under the metadata heading.

````kubectl get node {node_name} -o YAML````

If you have [jq](https://stedolan.github.io/jq/) installed, you can also get the information in the following way
````kubectl get node {node_name} -o json | jq .metadata.labels```
2. To filter this down to just labels.
````
kubectl get node {node_name} --show-labels=true

Modifier explanation
--show-labels=false: When printing, show all labels as the last column (default hide labels column)
````
We can now get all our nodes which share the same label, for example
````
kubectl get nodes --selector beta.kubernetes.io/arch=amd64

Modifier explanation
-l, --selector='': Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
````

3. See how labels are used by controllers for selection criteria in Kubernetes.
Services are a great example of using labels for selection criteria.

````kubectl get svc {svc_name} -n {namespace} -o yaml ````

If you've deployed [my pacman application](https://github.com/saintdle/pacman-tanzu) into Kubernetes, you can see this with the Load Balancer service for the web front end

````kubectl get svc pacman -n pacman -o yaml````

I've concatenated my output response below

````
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2021-06-03T17:41:29Z"
  finalizers:
  - service.kubernetes.io/load-balancer-cleanup
  labels:
    name: pacman
  managedFields:
.......
spec:
  clusterIP: 10.100.101.134
  clusterIPs:
  - 10.100.101.134
  externalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 31033
    port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    name: pacman
  sessionAffinity: None
  type: LoadBalancer
````

Create Labels on a node.

4. Adhoc add labels or annotations to an existing resource. From the Kubernetes [Cheet Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/).

````
kubectl label pods my-pod new-label=awesome                      # Add a Label
kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq       # Add an annotation
````

Follow the rest of the blog post I linked to for a deeper dive and explanations of when and where to use Labels and Annotations.