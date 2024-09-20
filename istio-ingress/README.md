# Istio Ingress

An example of setting up a cluster with an Istio gateway to allow external access
to the cluster.

What does this example do?

1. Creates a local k3d cluster for testing against a local Kubernetes cluster
2. Installs Istio on the cluster
3. Deploys a simple "hello" http application. This app responds to requests at the `/` path
   with a "Hello from <hostname>" message
4. Creates [an Istio gateway](https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/)
   that allows controlling what traffic is allowed into the cluster
5. Create [an Istio virtual service](https://istio.io/latest/docs/reference/config/networking/virtual-service/) to control routing of traffic in the cluster. It sets up a route externally accessible at [http://localhost:8080/hello](http://localhost:8080/hello)

## Running the example

If you have `make` installed, the easiest way to get up-and-running is via the following steps:

1. Create the cluster with Istio installed on it: you can run either `make` or `make install`
2. Install the Helm chart that sets up examples services wired up to Istio routing rules: `helm install istio-ingress ./`
3. After the Helm chart resources are created, go to [http://localhost:8080/hello](http://localhost:8080/hello). If everything worked, you'll see a "Hello from <hostname>" message. The hostname is the name of the underlying pod that responded to the message. If you refresh multiple times, you should see the hostname change from request-to-request

## Interesting notes

- The hello application was purposefully put in a different namespace than the Istio gateway. This mandates fully qualifying the destination in [the virtual service](./templates/istio-virtual-service.yaml) as opposed to many examples using short name. E.g. `hello.apps.svc.cluster.local` instead of just `hello`.
- The hello application only responds to requests to the `/` path. However, the [the virtual service](./templates/istio-virtual-service.yaml) exposes the path [http://localhost:8080/hello](http://localhost:8080/hello). This leverages [Istio's HTTP rewrite](https://istio.io/latest/docs/reference/config/networking/virtual-service/#HTTPRewrite) to send external requests for [http://localhost:8080/hello](http://localhost:8080/hello) to http://hello.apps.svc.cluster.local/ within the cluster.
