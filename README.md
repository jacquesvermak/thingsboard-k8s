# ThingsBoard PE Kubernetes Configuration

This repository contains production-ready Kubernetes YAML configurations for deploying ThingsBoard Professional Edition (PE) v4.0.1 with complete infrastructure components.

## 🏗️ Architecture

- **ThingsBoard PE v4.0.1** - Monolith core service
- **CoAP Transport** - 4 replicas with DTLS support (port 5684)
- **HTTP/MQTT Transports** - Separate transport services
- **Cassandra** - Primary database storage
- **Kafka** - Message queue system
- **Redis** - Caching and session storage
- **Zookeeper** - Kafka coordination

## 📁 Repository Structure

```
├── values.yaml                    # Main Helm configuration file
├── configmaps/                    # Application configurations
├── deployments/                   # Deployment manifests
├── statefullsets/                 # StatefulSet configurations
├── services/                      # Service definitions
├── hpa/                          # Horizontal Pod Autoscaler configs
├── secrets/                      # Secret and certificate configs
├── reassignment_4_replicas/      # Kafka partition reassignment
└── reassignment_js_eval_requests/ # JS executor reassignment
```

## 🔐 DTLS Configuration

This configuration includes complete DTLS (Datagram Transport Layer Security) support for CoAP transport:

- **DTLS Endpoint**: Enabled on port 5684
- **Certificate Management**: Via `coap-pem-configmap`
- **Device Authentication**: Secure DTLS connections supported
- **LoadBalancer**: External endpoint configuration included

## 🚀 Deployment

### Prerequisites
- Kubernetes cluster (tested on EKS)
- Helm 3.x
- kubectl configured for your cluster

### Quick Deploy
```bash
# Deploy with Helm
helm install thingsboard-pe . -f values.yaml -n thingsboard-qa

# Or apply individual manifests
kubectl apply -f configmaps/
kubectl apply -f secrets/
kubectl apply -f statefullsets/
kubectl apply -f deployments/
kubectl apply -f services/
kubectl apply -f hpa/
```

## ⚙️ Configuration Notes

### Production Readiness
- **QA Configuration**: Currently configured for QA environment
- **Namespace**: Uses `thingsboard-qa` namespace
- **Redis**: Points to localhost (update for production)

### For Production Deployment
Update the following in `values.yaml`:
1. Change namespace from `thingsboard-qa` to production namespace
2. Update Redis host from `localhost` to production Redis endpoint
3. Update domain/certificate configurations as needed

## 🔧 Key Features

- ✅ **DTLS Support**: Complete CoAP DTLS configuration
- ✅ **High Availability**: Multiple replicas and auto-scaling
- ✅ **Resource Management**: Proper CPU/memory limits
- ✅ **Monitoring**: HPA configurations included
- ✅ **Security**: Certificates and secrets management

## 📊 Resource Requirements

- **ThingsBoard Node**: 30-32GB memory
- **CoAP Transport**: 8GB memory per replica (4 replicas)
- **Cassandra**: 32GB memory
- **Kafka**: Configured for high throughput

## 🐛 Troubleshooting

### Common Issues
1. **DTLS Certificate Issues**: Ensure `coap-pem-configmap` is properly mounted
2. **Kafka Connectivity**: Check partition reassignment configurations
3. **Device Connectivity**: Verify LoadBalancer endpoint and port 5684 access

### Logs
```bash
# Check transport logs
kubectl logs -f thingsboard-coap-transport-0 -n thingsboard-qa

# Check core service logs
kubectl logs -f thingsboard-node-0 -n thingsboard-qa
```

## 📝 License

This configuration is for ThingsBoard Professional Edition. Ensure you have appropriate licensing.

## 🤝 Contributing

1. Test changes in staging environment first
2. Update both individual YAML files and values.yaml
3. Verify DTLS functionality after changes
4. Document any configuration changes

---

**Note**: This configuration successfully resolves device telemetry connectivity issues and maintains stable DTLS connections for IoT devices.