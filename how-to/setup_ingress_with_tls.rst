JAAS: Setup Ingress with TLS
============================
The NGINX Ingress Integrator is a charm responsible for creating Kubernetes ingress rules, 
these rules can be hardened via TLS and the charm provides a means to do so. See `here <https://charmhub.io/nginx-ingress-integrator>`__.

Our LEGO charms provide certificates for charms from a desired ACME server and can be integrated
with the integrator to enable TLS at the ingress level. See `here <https://charmhub.io/httprequest-lego-k8s>`__.

You will require a domain that your ACME is aware of and an NGINX ingress controller installed
on your Kubernetes cluster.

With JAAS deployed, you can deploy both LEGO and the integrator, and integrate your LEGO charm deployment
to your ingress integrator, and then the ingress integrator to JIMM to enable TLS ingress for your deployment.