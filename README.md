# Entando Quickstart using Helm

This quickstart uses Entando's standard Wildfly image.

# Prerequisites

  - You have installed either the Openshift client oc or the Kubernetes client kubectl locally
  - You have access to an Openshift or Kubernetes cluster, with a sandbox namespace/project you have admin access to. In subsequent instructions we will refer to this namespace as '[your-sandbox-namespace]'
  - The Entando K8S Custom Resource Definitions have been registered on your Kubernetes Cluster. For more information, you can consult these [instructions](https://github.com/entando-k8s/entando-k8s-custom-model/blob/master/src/main/resources/crd/README.md).
  - You have installed Helm, ideally the Helm 2 client without Tiller


# Steps to deploy to Openshift/Kubernetes

1. In the `values.yaml` file you need to update the configuration for your environment and deployment.

    - If you are deploying in Openshift, set the operator.supportOpenshift = true , otherwise set it to `false`
    - Set `ENTANDO_DEFAULT_ROUTING_SUFFIX` to the value that matches the env you're going to deploy to. For local clusters (MicroK8S, Minikube) that would be your local IP address with the suffix `nip.io`, e.g. `192.168.1.9.nip.io`. On a shared cluster you may need to consult your cluster admin.
    - If you want to deploy the Process Driven Applications Plugin (PDA), set `deployPDA = true`, otherwise leave it as false
    - Embedded databases are used by default for the Entando Composite App - i.e. EntandoKeycloakServer, EntandoClusterInfrastructure and EntandoApp; if you want to switch to another DMBS you can change the property `app.dbms`. Accepted values are: `none` (default), `postgresql`, `mysql`, `oracle`,
    - If you want to impose limits on the minimum/maximum requested resources you can set to `true` the `ENTANDO_K8S_OPERATOR_IMPOSE_DEFAULT_LIMITS`. You can find more info [here](https://dev.entando.org/docs/getting-started/#add-custom-resources).

2. Ensure you have namespace in the cluster you can use, and the current user has admin permissions on the namespace.
3. Make sure the dependency have been uploaded and are up-to-date
```
helm dependency update .
```
4. Process the template and deploy the output using your favorite Kubernetes client, e.g:
```
helm template --name=my-app  --namespace=[your-sandbox-namespace] ./ | kubectl create -f -
```

If desired you can apply a more granular approach regarding used DBMSs for the Composite App components. In such a case you can follow all the previous step except the 7 and proceed as follows:

5. Process the template:
```
helm template --name=my-app  --namespace=[your-sandbox-namespace] ./ > generated-template.yml
```

6. Deploy the generated template file using your favorite Kubernetes client, e.g:
```
kubectl create -f generated-template.yml
```
