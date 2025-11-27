# kube9-ui

Web-based user interface for the kube9 ecosystem. Provides browser-based access to Kubernetes cluster management and kube9 operator features.

## Overview

kube9-ui is an open-source web interface that complements the kube9-vscode extension. It provides the same functionality in a browser-based interface, enabling users who don't use VS Code to access kube9 operator outputs, insights, and cluster management features.

**Installation Model**: Like ArgoCD, kube9-ui is installed directly into your Kubernetes cluster via Helm chart. It's private by default (accessible via port-forward), but can be exposed publicly if desired.

## Features

### Free Tier
- **Cluster Management**: Tree view navigation, resource viewer, YAML editor
- **Multi-Cluster Support**: Manage multiple clusters and contexts
- **Real-Time Updates**: Watch API integration for live updates
- **ArgoCD Integration**: View sync status and deployment history
- **Resource Visualization**: Relationship graphs and status indicators
- **Pod Logs & Events**: Built-in log viewer and event timeline

### Pro Tier (with kube9-operator)
- **AI Insights**: Display insights from OperatorStatus CRD
- **Predictive Analytics**: Performance and cost optimization recommendations
- **Security Analysis**: Security posture visualization
- **On-Demand Analysis**: Request analysis via operator

## Quick Start

### Installation

```bash
# Add the kube9 Helm repository
helm repo add kube9 https://charts.kube9.io

# Install kube9-ui
helm install kube9-ui kube9/kube9-ui \
  --namespace kube9-system \
  --create-namespace
```

### Access

**Private Access (Default)**:
```bash
kubectl port-forward -n kube9-system svc/kube9-ui 8080:80
# Access at http://localhost:8080
```

**Public Access** (optional):
```bash
helm upgrade kube9-ui kube9/kube9-ui \
  --namespace kube9-system \
  --set service.type=LoadBalancer \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=kube9-ui.example.com
```

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Your Kubernetes Cluster                   │
│                                                             │
│  ┌─────────────────┐      ┌─────────────────────────────┐  │
│  │   kube9-ui      │      │     kube9-operator          │  │
│  │   (Web UI)      │─────▶│     (Optional)              │  │
│  └────────┬────────┘      └─────────────────────────────┘  │
│           │                            │                    │
│           │                            ▼                    │
│           │               ┌─────────────────────────────┐  │
│           │               │   OperatorStatus CRD        │  │
│           │               │   (Shared Data Source)      │  │
│           │               └─────────────────────────────┘  │
│           │                                                │
│           ▼                                                │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              Kubernetes API Server                   │  │
│  └─────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

- **kube9-ui** reads from OperatorStatus CRD (same as kube9-vscode)
- **Works with or without** kube9-operator installed
- **Private by default** - you control exposure

## Configuration

### Helm Values

| Value | Description | Default |
|-------|-------------|---------|
| `service.type` | Service type (ClusterIP, NodePort, LoadBalancer) | `ClusterIP` |
| `ingress.enabled` | Enable Ingress | `false` |
| `rbac.create` | Create RBAC resources | `true` |
| `operatorNamespace` | Namespace where operator is installed | `kube9-system` |
| `resources.requests.memory` | Memory request | `128Mi` |
| `resources.requests.cpu` | CPU request | `100m` |

### Security

kube9-ui follows the ArgoCD model - **security is your responsibility**:

- **No default authentication** - add your own auth layer (OAuth, OIDC, etc.)
- **Private by default** - exposed only via port-forward
- **RBAC integration** - uses Kubernetes RBAC via ServiceAccount
- **TLS/HTTPS** - configure via Ingress or LoadBalancer

## Development

### Prerequisites

- Node.js 22+
- npm 10+
- Access to a Kubernetes cluster (for testing)

### Setup

```bash
# Clone the repository
git clone https://github.com/alto9/kube9-ui.git
cd kube9-ui

# Install dependencies
npm install

# Start development server
npm run dev
```

### Build

```bash
# Build for production
npm run build

# Build Docker image
docker build -t kube9-ui .
```

## Technology Stack

- **Framework**: React with TypeScript
- **Kubernetes Client**: @kubernetes/client-node
- **Styling**: Tailwind CSS
- **Build**: Vite
- **Container**: Docker with Node.js runtime

## Related Projects

- [kube9-operator](https://github.com/alto9/kube9-operator) - Core Kubernetes operator
- [kube9-vscode](https://github.com/alto9/kube9-vscode) - VS Code extension

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Security

For security vulnerabilities, please see [SECURITY.md](SECURITY.md).

## License

MIT License - see [LICENSE](LICENSE) for details.

## Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/alto9/kube9-ui/issues)
- **GitHub Discussions**: [Ask questions and share ideas](https://github.com/alto9/kube9-ui/discussions)

---

**Built with ❤️ by [Alto9](https://alto9.com) - Making Kubernetes management accessible to everyone**

