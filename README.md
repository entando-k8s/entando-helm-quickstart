# Entando Quickstart using Helm

This quickstart uses Entando's standard Wildfly image.

# Steps to deploy to Openshift/Kubernetes

In the values.yaml file you need to update the configuration for your environment and deployment.

1. If you are deploying in Openshift, set the operator.supportOpenshift = true , otherwise set it to false
2. Set `ENTANDO_DEFAULT_ROUTING_SUFFIX` to the value that matches the env you're going to deploy to. For local clusters (MicroK8S, Minikube) that would be your local IP address with the suffix `nip.io`, e.g. `192.168.1.9.nip.io`. On a shared cluster you may need to consult your cluster admin.
3. Ensure you have namespace in the cluster you can use, and the current user has admin permissions on the namespace.
4. Set the value of `ENTANDO_K8S_OPERATOR_SCOPE` to the same namespace where you're going to create your app
4. Process the template and deploy the output using your favorite Kubernetes client, e.g:
               `helm template --name=my-app  --namespace=my-namespace ./ | kubectl create -f -`

