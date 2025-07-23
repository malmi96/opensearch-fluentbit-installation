OpenSearch Installation - 

https://github.com/opensearch-project/opensearch-k8s-operator/blob/main/docs/userguide/main.md

1. Add the helm repo: `helm repo add opensearch-operator https://opensearch-project.github.io/opensearch-k8s-operator/`
2. Install the Operator: `helm install opensearch-operator opensearch-operator/opensearch-operator`
3. kubectl apply -f my-first-cluster.yaml  (see the my-first-cluster.yaml)

FluentBit Installation -

helm repo add fluent https://fluent.github.io/helm-charts

helm install fluent-bit fluent/fluent-bit

SSL / TLS Issue in OpenSearch -

#Extract the CA certificate from OpenSearch

kubectl exec -it opensearch-node-0 -- cat /usr/share/opensearch/config/root-ca.pem

#Create a configmap using the crt

kubectl create configmap opensearch-ca-cert --from-file=opensearch-ca.crt=/Users/malmi/opensearch-ca.crt -n logging

#edit daemonset of fluentbit

volumeMounts:
  - name: tls-certs
    mountPath: /fluent-bit/tls
    readOnly: true

volumes:
  - name: tls-certs
    configMap:
      name: opensearch-ca-cert


Add the configmap of fluentbit - (see the fluentbit-configmap.yaml)


