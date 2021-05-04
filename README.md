# Entando Quickstart using Helm

This quickstart uses Entando's standard Wildfly image.

# Prerequisites

  - You have installed either the Openshift client oc or the Kubernetes client kubectl locally
  - You have access to an Openshift or Kubernetes cluster, with a sandbox namespace/project you have admin access to. In subsequent instructions we will refer to this namespace as '[your-sandbox-namespace]'
  - You have installed Helm, ideally the Helm 2 client without Tiller
  - You have read/write access to certain cluster resources such as ClusterRoles and CustomResourceDefinitions, or alternatively someone with such access can perform operations on your behalf

# Steps to deploy to Openshift/Kubernetes

1. Deploy the correct distribution of the Entando operator from the entando-k8s-operator-bundle project (to be performed by someone with above mentioned cluster read/write access)
   
   - for Openshift 3.11: 
      
        `oc apply -n [your-sandbox-namespace] -f https://raw.githubusercontent.com/entando-k8s/entando-k8s-operator-bundle/v6.3.2/manifests/k8s-before-116/namespace-scoped-deployment/all-in-one.yaml`
   - for Openshift 4.x please use the [Operator Hub](https://link.to.oh.tutorial)
   - for Kubernetes clusters version 1.16 and later:
       
        `kubectl apply -n [your-sandbox-namespace] -f https://raw.githubusercontent.com/entando-k8s/entando-k8s-operator-bundle/v6.3.2/manifests/k8s-116-and-later/namespace-scoped-deployment/all-in-one.yaml`
   - for Kubernetes clusters before version 1.16 (deprecated):
     
        `kubectl apply -n [your-sandbox-namespace] -f https://raw.githubusercontent.com/entando-k8s/entando-k8s-operator-bundle/v6.3.2/manifests/k8s-before-116/namespace-scoped-deployment/all-in-one.yaml`
     
2. Configure the Entando Operator (optional)
   
   The Entando Operator can be configured by modifying the values of properties in the entando-operator-config ConfigMap that can be created in the same namespace as
   the operator which would be [your-sandbox-namespace]. You can create it from the sample available in sample-configmaps/entando-operator-config.yaml, e.g.:
      
      `kubect apply -f sample-configmaps/entando-operator-config.yaml -n [your-sandbox-namespace]`
   
   You can edit the configmap in your local text editor using the 'edit' command e.g.:
   
      `kubectl edit configmap entando-operator-config -n [you-sandbox-namespace]`
   
   Please consult the comments in the sample-configmaps/entando-operator-config.yaml file for more details on common 
   configuration options.
3. Customize some of the settings pertaining to the Entando App that will be deployed in the  `values.yaml` file.
   
   Please consult the comments in the values.yaml file for more details on what you can change. Please note that 
   most of these settings can be set directly on the Entando custom resource after deployment too.

4. Process the template and deploy the output using your favorite Kubernetes client, e.g:
   
      `helm template --name=quickstart  ./ | kubectl apply -n [your-sandbox-namespace] -f -`

5. Follow the progress of the Entando app deployment process using

     `watch kubectl get pods -n [your-sandbox-namespace]`
