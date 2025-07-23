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


