# Istio Ingress

Simple Helm chart setting up a cluster using an Istio ingress controller to allow external access
to the cluster

## Running the example

If you have `make` installed, the easiest way to get up-and-running is via the following steps:

1. Create the cluster with Istio installed on it: you can run either `make` or `make install`
2. Install the Helm chart that sets up examples services wired up to Istio routing rules: `helm install istio-ingress ./`
