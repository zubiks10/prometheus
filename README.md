# prometheus

Installing Prometheus in a Kubernetes cluster is a straightforward process. Below is a step-by-step guide to set it up using Helm, which simplifies the deployment and management of applications on Kubernetes. 

### Prerequisites

1. **Kubernetes Cluster**: Ensure you have access to a running Kubernetes cluster (Minikube, GKE, EKS, etc.).
2. **kubectl**: Ensure you have `kubectl` installed and configured to communicate with your cluster.
3. **Helm**: Install Helm if you haven't already. You can follow the installation guide from the [official Helm documentation](https://helm.sh/docs/intro/install/).

### Step 1: Add the Prometheus Helm Chart Repository

Open your terminal and run the following commands to add the Prometheus community chart repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Step 2: Install Prometheus

You can install Prometheus using the Helm chart with default configurations. To do this, run:

```bash
helm install prometheus prometheus-community/prometheus
```

This command deploys Prometheus in your Kubernetes cluster. It will create several resources, including a Service, StatefulSet, ConfigMap, and more.

### Step 3: Access Prometheus UI

By default, Prometheus will not be exposed to the outside world. To access the Prometheus UI, you can either use port forwarding or expose it via a LoadBalancer or NodePort service.

#### Option A: Port Forwarding

Run the following command to port forward the Prometheus server:

```bash
kubectl port-forward svc/prometheus-server 9090:80
```

Now you can access the Prometheus UI at [http://localhost:9090](http://localhost:9090).

#### Option B: Expose via LoadBalancer or NodePort

If you prefer to expose Prometheus using a LoadBalancer or NodePort, you can modify the service type:

1. **Edit the Prometheus service**:

```bash
kubectl edit svc prometheus-server
```

2. **Change the type from `ClusterIP` to `LoadBalancer` or `NodePort`**. 

3. **Save and exit**.

### Step 4: Configure Scraping

By default, Prometheus scrapes metrics from itself. You can add other targets by modifying the `values.yaml` file or using the Helm install command with additional configuration.

For example, if you have other services in your cluster that expose metrics, you can add their endpoints to the `scrape_configs` section in the Prometheus configuration.

### Step 5: Verify the Installation

You can check the status of your Prometheus deployment using:

```bash
kubectl get pods -n default -l app=prometheus
```

### Step 6: Clean Up

To uninstall Prometheus and clean up resources, run:

```bash
helm uninstall prometheus
```

### Additional Resources

- [Prometheus Documentation](https://prometheus.io/docs/introduction/overview/)
- [Helm Documentation](https://helm.sh/docs/)

This should help you get Prometheus up and running in your Kubernetes cluster. If you need further customization or have any issues, feel free to ask!