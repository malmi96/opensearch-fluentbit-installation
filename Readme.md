# OpenSearch and Fluent Bit Setup on Kubernetes

This guide provides instructions for installing OpenSearch using the OpenSearch Kubernetes Operator and configuring Fluent Bit to send logs to OpenSearch with SSL/TLS support.

---

## 🔧 OpenSearch Installation

Follow the official user guide for detailed instructions:  
📘 [OpenSearch Operator User Guide](https://github.com/opensearch-project/opensearch-k8s-operator/blob/main/docs/userguide/main.md)

### Steps:

1. **Add the Helm repository:**
   ```bash
   helm repo add opensearch-operator https://opensearch-project.github.io/opensearch-k8s-operator/
   helm install opensearch-operator opensearch-operator/opensearch-operator
   kubectl apply -f my-first-cluster.yaml
## Fluent Bit Installation
Add the Fluent Helm chart repo:
   ```bash
   helm repo add fluent https://fluent.github.io/helm-charts
   helm install fluent-bit fluent/fluent-bit
```
## 🔐 SSL/TLS Configuration for OpenSearch
To enable secure communication between Fluent Bit and OpenSearch:

1. **Extract the CA certificate from the OpenSearch pod:**
   ```bash
   kubectl exec -it opensearch-node-0 -- cat /usr/share/opensearch/config/root-ca.pem
3. **Create a ConfigMap with the certificate:**
   ```bash
   kubectl create configmap opensearch-ca-cert --from-file=opensearch-ca.crt=/Users/malmi/opensearch-ca.crt -n logging
4. **Update the Fluent Bit DaemonSet with volume mounts:**
   Add the following to the volumeMounts section:
  ```yaml
- name: tls-certs
  mountPath: /fluent-bit/tls
  readOnly: true
```
   Add the following under volumes:
   ```yaml
   - name: tls-certs
  configMap:
    name: opensearch-ca-cert
```
---

## 🛠 Fluent Bit ConfigMap

Include your Fluent Bit configuration in a Kubernetes `ConfigMap`.  
You can use or customize the example provided in the file: `fluentbit-configmap.yaml`.

This config should define Fluent Bit’s output settings, such as connection details to OpenSearch, index naming, and TLS certificate path.

> Make sure your DaemonSet is updated to mount the required TLS volume as previously explained.

---

## 📁 Files to Prepare

- `my-first-cluster.yaml` – Defines the OpenSearch cluster specification.
- `fluentbit-configmap.yaml` – Contains Fluent Bit output configuration including secure connection (TLS) to OpenSearch.

---

## 📌 Notes

- Ensure the `logging` namespace (or your desired namespace) exists before applying resources.
  ```bash
  kubectl create namespace logging


