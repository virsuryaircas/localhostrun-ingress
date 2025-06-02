# localhost.run as Kubernetes Ingress Integration

A Kubernetes manager that provides ingress-like functionality using localhost.run tunneling service. While not a traditional ingress controller, it acts as an ingress by automatically creating SSH reverse tunnels to expose your Kubernetes services to the internet through localhost.run.

## 🚀 Features

- **Automatic Tunnel Management**: Monitors ingresses and creates SSH tunnels automatically
- **Health Monitoring**: Continuously checks tunnel health and recreates dead tunnels
- **Dynamic URL Updates**: Updates ingress specifications with generated [localhost.run](https://localhost.run) URLs
- **Zero Configuration**: Works out of the box with localhost.run's no-key access
- **Resilient**: Handles tunnel failures gracefully with automatic recovery

## 🏗️ How It Works

This solution acts as a pseudo-ingress controller that:

- Watches for Kubernetes ingresses with `ingressClassName: localhostrun`
- Extracts service information (name, port, NodePort)
- Creates SSH reverse tunnels to [localhost.run](https://localhost.run)
- Updates the ingress with the generated public URL
- Monitors tunnel health and recreates as needed

Note: This is not a traditional ingress controller but provides similar functionality through tunneling.

## ⚙️ Setup Instructions

```bash
# Clone the repository
git clone https://github.com/virsuryaircas/localhostrun-ingress.git

# Navigate to the project directory
cd localhostrun-ingress

# Create the 'lhr-ingress' namespace
kubectl create ns lhr-ingress

# Apply all the manifests
kubectl apply -k .

# Verify the pods in the 'lhr-ingress' namespace
kubectl get po -n lhr-ingress

# Check available ingress classes
kubectl get ingressclass

# Navigate to the examples directory and apply example manifests
cd examples
kubectl apply -f hellworld-deploy.yaml
kubectl apply -f helloworld-svc.yaml
kubectl apply -f helloworld-ingress.yaml

# Verify the ingress
kubectl get ingress

#Output
NAME           CLASS          HOSTS                     ADDRESS           PORTS   AGE
helloworld     localhostrun   xxxxxxxxxxxxxx.lhr.life                     80      10s

```

> **NOTE:** The service type must be `NodePort` to expose it using LocalhostRun Ingress.

## 📝 Docker Image Changelog

### Version 1
- Initial release (inception)

### Version 2
- Fixed updating ingress hosts when tunnels restart with new domains

## Credits:

Special thanks to [localhost.run](https://localhost.run) and their [GitHub repository](https://github.com/localhost-run)  
for providing subdomain tunneling services that power this project.
