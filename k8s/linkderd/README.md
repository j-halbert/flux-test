# Linkerd Manual Install

Consult the [Getting Started](https://linkerd.io/2/getting-started/) page for the most recent information on installing Linkerd in Kubernetes.
This walks through setting up Linkerd 2.

## Automating Install

I haven't worked this piece out yet.  I'm hoping something in `helm` will help me get there.  Otherwise, installation may be a semi-manual process.

The `linkerd` CLI generates `yaml` configuration files to install the control and data planes to a Kubernetes installation.  This _might_ be the way to go, but locks the installation into a specific version (which _might_ not be the worst idea, I just have to think further on how we're going to maintain the cluster, etc. with a view to upgrades).

## Automating Data Plane Mesh Sidecar Injection

During installation, the `---proxy-auto-inject` option can be passed, which will ensure that pods automatically have the data plane mesh sidecar injected into them.  Depending on the order in which things are done during the initial setup of the cluster, it may be necessary to modify running pods to get the sidecar injected.  This is due to constraints with the [admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#admission-webhooks) API provided by Kubernetes.

Optionally, auto-injection can be enabled after the fact by passing the above option to `linkerd upgrade`.

### Configuration

Linkerd only injects the mesh sidecar either on pods with the `linkerd.io/inject: enabled` annotation in the pod specification, or for pods in namespaces with the `linkerd.io/inject: enabled` annotation.  For namespaces where auto-injection has been enabled through the annotation, injection _may_ be disabled on a pod-by-pod basis with the `linkerd.io/inject: disabled` annotation on the pod's spec.

For our project, we want auto-injection at the namespace level.  No pods should be deployed with injection disabled without a significant reason, and documentation to this fact within the service's `README`.

## Monitoring Semantics

***NOTE*** 1xx-4xx status codes look okay to Linkerd.  Only 5xx codes show as failure.  Maybe this is configurable, but it also might be a little misleading.
