---
title: How can services that use Istio access non-Istio services?
weight: 40
---

When mutual TLS is globally enabled, the *global* destination rule matches all services in the cluster, whether or not these services have an Istio sidecar.
This includes the Kubernetes API server, as well as any non-Istio services in the cluster. To communicate with such services from services that have an Istio
sidecar, you need to set a destination rule to exempt the service. For example:

{{< text bash >}}
$ cat <<EOF | istioctl create -f -
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
 name: "api-server"
spec:
 host: "kubernetes.default.svc.cluster.local"
 trafficPolicy:
   tls:
     mode: DISABLE
EOF
{{< /text >}}

> This destination rule is already added to the system as part of the
[Istio installation with default mutual TLS](/docs/setup/kubernetes/quick-start/#option-2-install-istio-with-default-mutual-tls-authentication).

Similarly, you can add destination rules for other non-Istio services. For more examples, see [task](/docs/tasks/security/authn-policy/#request-from-istio-services-to-non-istio-services).