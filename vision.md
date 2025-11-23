# kube9-ui - Vision

## Mission

kube9-ui is an open-source web-based user interface for the kube9 ecosystem. It provides the same functionality as kube9-vscode but in a browser-based interface, enabling users who don't use VS Code to access kube9 operator outputs, insights, and cluster management features. Like ArgoCD UI, kube9-ui is installed directly into the user's Kubernetes cluster, providing a private-by-default interface that users can expose publicly if desired.

## Core Purpose

**Why kube9-ui exists**: The kube9-operator is the core component that does the major work - collecting metrics, managing insights, and enabling Pro tier features. However, kube9-vscode only serves VS Code users. For users who don't use VS Code but want to access operator outputs and insights, there was previously no interface available. kube9-ui fills this gap by providing a web-based interface that reads from the same OperatorStatus CRD that the VS Code extension uses, ensuring feature parity and seamless operator integration.

**Installation Philosophy**: kube9-ui follows the ArgoCD model - users install it directly into their Kubernetes cluster via a separate Helm chart. It's private by default (accessible via port-forward or within cluster), but users can expose it publicly (Ingress, LoadBalancer) if desired. Security configuration is left entirely to the user, just like ArgoCD.

## Long-Term Vision

### The Web Interface We're Building

kube9-ui will become the **standard web-based interface for kube9**, providing:

1. **Browser-Based Access**: Full-featured Kubernetes cluster management from any web browser
2. **Operator Integration**: Seamless integration with kube9-operator via OperatorStatus CRD
3. **Feature Parity**: Same functionality as kube9-vscode, adapted for web interface
4. **Multi-Cluster Support**: Manage multiple clusters and contexts from a single interface
5. **Progressive Enhancement**: Free tier provides powerful features, Pro tier unlocks AI insights
6. **ArgoCD Integration**: Seamless integration with ArgoCD for GitOps visibility (free tier)

### Strategic Goals

**Short-Term (6-12 months)**
- Launch MVP with core cluster management features
- Provide Helm chart for easy installation
- Achieve feature parity with kube9-vscode for free tier features
- Build responsive, mobile-friendly interface
- Establish installation and usage documentation

**Medium-Term (1-2 years)**
- Add Pro tier AI insights display and on-demand analysis
- Enhance ArgoCD integration with advanced GitOps features
- Build plugin system for extensibility
- Add team collaboration features (shared views, annotations)
- Support for custom themes and branding

**Long-Term (2+ years)**
- Become the standard web interface for kube9 ecosystem
- Enable advanced multi-cluster federation views
- Build integration marketplace for third-party plugins
- Provide enterprise features (SSO, RBAC, audit logging)
- Support for edge clusters and air-gapped environments

## Key Principles

### 1. Operator-Centric Architecture
- kube9-operator is the core - kube9-ui is an interface layer
- All data comes from OperatorStatus CRD (same as VS Code extension)
- No direct server communication - operator handles all external communication
- Works with or without Pro tier (graceful degradation)

### 2. ArgoCD Installation Model
- **Private by Default**: Installed in cluster, accessible via port-forward or cluster networking
- **User-Controlled Exposure**: Users can expose publicly via Ingress/LoadBalancer if desired
- **Security Left to User**: No default authentication - users add their own auth layer
- **Cluster-Native**: Runs as a standard Kubernetes Deployment with Service

### 3. Feature Parity with kube9-vscode
- Same core functionality, adapted for web interface
- Reads from same OperatorStatus CRD
- Supports same free tier and Pro tier features
- Consistent user experience across interfaces

### 4. Design System Consistency
- Uses martex-template design system (same as kube9-portal)
- Template copied (never modified) from `alto9-docs/marketing/martex-template`
- Ensures consistent design across kube9 ecosystem
- Professional, modern UI that matches kube9 branding

### 5. Performance & Reliability
- Fast, responsive UI even with large clusters
- Efficient Kubernetes API usage (watch, caching)
- Graceful degradation when operator unavailable
- Minimal resource footprint (lightweight deployment)

### 6. Open Source & Community
- Open source project under Alto9 organization
- Community contributions welcome
- Clear documentation and contribution guidelines
- Transparent development process

## Installation & Deployment

### Helm Chart Approach

kube9-ui is distributed as a **separate Helm chart** (`kube9-ui`) that can be installed independently of kube9-operator. This separation provides:

- **Independent Installation**: Users can install kube9-ui without operator (basic kubectl features)
- **Flexible Deployment**: Install UI in different namespace or cluster than operator
- **Version Independence**: UI and operator can be updated independently
- **Resource Isolation**: UI and operator have separate resource requirements

### Installation Patterns

**1. Private Installation (Default)**
```bash
helm repo add kube9 https://charts.kube9.io
helm install kube9-ui kube9/kube9-ui \
  --namespace kube9-system \
  --create-namespace
```

Access via port-forward:
```bash
kubectl port-forward -n kube9-system svc/kube9-ui 8080:80
# Access at http://localhost:8080
```

**2. Public Installation (User Choice)**
```bash
helm install kube9-ui kube9/kube9-ui \
  --namespace kube9-system \
  --create-namespace \
  --set service.type=LoadBalancer \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=kube9-ui.example.com
```

**3. Behind Authentication (User Adds)**
Users can add their own authentication layer (OAuth, OIDC, etc.) via Ingress annotations or sidecar proxy.

### Helm Chart Architecture

**Chart Structure:**
```
kube9-ui/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml      # kube9-ui Deployment
    ├── service.yaml        # Service (ClusterIP by default)
    ├── ingress.yaml        # Optional Ingress (disabled by default)
    ├── serviceaccount.yaml # ServiceAccount for Kubernetes API access
    ├── rbac.yaml           # RBAC for reading OperatorStatus CRD
    └── configmap.yaml      # Optional configuration
```

**Key Helm Values:**
- `service.type`: ClusterIP (default), NodePort, or LoadBalancer
- `ingress.enabled`: Enable Ingress (false by default)
- `rbac.create`: Create RBAC resources (true by default)
- `operatorNamespace`: Namespace where operator is installed (kube9-system)
- `resources`: Resource requests/limits for UI pod

### Relationship to kube9-operator

**Independent but Complementary:**
- kube9-ui can be installed without operator (provides basic kubectl features)
- kube9-ui reads from OperatorStatus CRD when operator is present
- Both can be installed via separate Helm charts
- Both can be in same namespace (kube9-system) or different namespaces

**Installation Scenarios:**

1. **Operator Only**: User installs operator, uses VS Code extension
2. **UI Only**: User installs UI, gets basic cluster management (no operator features)
3. **Both**: User installs both, gets full feature set via UI
4. **Operator + VS Code**: User installs operator, uses VS Code extension (current model)

## Technology Stack

### Frontend
- **Framework**: React or Next.js with TypeScript
- **Kubernetes Client**: @kubernetes/client-node (for direct API access)
- **State Management**: React Context or Zustand
- **UI Framework**: Based on martex-template design system
- **Styling**: Tailwind CSS (aligned with martex-template)
- **Build**: Vite or Next.js build system

### Backend (If Needed)
- **API Layer**: Optional Next.js API routes or standalone Node.js server
- **Kubernetes Integration**: Direct Kubernetes API access via ServiceAccount
- **Authentication**: Optional - user can add their own auth layer

### Deployment
- **Container**: Docker image with Node.js runtime
- **Orchestration**: Kubernetes Deployment
- **Service**: Kubernetes Service (ClusterIP by default)
- **Distribution**: Helm chart via charts.kube9.io

## Feature Set

### Free Tier Features

**Core Cluster Management:**
- Tree view navigation of Kubernetes resources
- Resource viewer and YAML editor
- Multi-cluster and multi-context support
- Resource creation, editing, and deletion
- Real-time updates via Kubernetes watch API
- Namespace filtering and search

**ArgoCD Integration:**
- View ArgoCD Applications and sync status
- Monitor drift detection
- View deployment history
- Basic GitOps visibility

**Visual Features:**
- Resource relationship visualization
- Pod logs viewer
- Event timeline
- Resource status indicators

### Pro Tier Features (When Operator Present)

**AI Insights:**
- Display insights from OperatorStatus CRD
- Insight filtering and search
- Insight details and recommendations
- Acknowledge/dismiss insights
- On-demand analysis requests (via operator)

**Advanced Features:**
- Predictive analytics visualization
- Cost optimization recommendations
- Security posture analysis
- Performance trend analysis

## User Experience

### Access Patterns

**1. Private Access (Default)**
- Installed in cluster, accessible via port-forward
- Perfect for local development and testing
- No external exposure, maximum security

**2. Cluster-Internal Access**
- Accessible from within cluster network
- Useful for team members with cluster access
- Can be exposed via ClusterIP Service

**3. Public Access (User Choice)**
- Exposed via Ingress or LoadBalancer
- User adds authentication layer
- Useful for public dashboards or team access

### Responsive Design
- Works on desktop, tablet, and mobile devices
- Touch-friendly interface for mobile access
- Responsive layouts adapt to screen size
- Progressive enhancement for features

### Performance
- Fast initial load (< 2 seconds)
- Efficient resource loading (lazy loading)
- Optimized Kubernetes API calls (batching, caching)
- Real-time updates without page refresh

## Security Considerations

### User Responsibility Model

**Like ArgoCD, security is left to the user:**

1. **No Default Authentication**: UI has no built-in auth - users add their own
2. **RBAC Integration**: Uses Kubernetes RBAC via ServiceAccount
3. **Network Security**: Private by default, user controls exposure
4. **TLS/HTTPS**: User configures via Ingress or LoadBalancer

### Security Best Practices

**Documentation will provide guidance for:**
- Adding authentication (OAuth, OIDC, basic auth)
- Configuring TLS/HTTPS
- Setting up network policies
- Using RBAC for access control
- Securing public deployments

### RBAC Requirements

kube9-ui requires RBAC permissions to:
- Read OperatorStatus CRD (when operator present)
- Read Kubernetes resources (for cluster management)
- Watch resources (for real-time updates)
- Create/update/delete resources (for management operations)

## Relationship to Other Components

### kube9-operator
- **Core Dependency**: kube9-ui reads from OperatorStatus CRD created by operator
- **Optional**: UI can work without operator (basic features only)
- **Separate Charts**: Both distributed as separate Helm charts
- **Same Namespace**: Typically installed in same namespace (kube9-system)

### kube9-vscode
- **Complementary**: VS Code extension and kube9-ui serve different user preferences
- **Feature Parity**: Both provide same functionality, different interfaces
- **Shared Foundation**: Both read from OperatorStatus CRD
- **User Choice**: Users choose interface based on their workflow

### kube9-portal
- **Different Purpose**: Portal is for account management, kube9-ui is for cluster management
- **Shared Design**: Both use martex-template design system
- **No Direct Integration**: kube9-ui is standalone, doesn't require portal
- **Pro Tier**: Portal manages API keys, UI displays insights from operator

### kube9-server
- **No Direct Connection**: kube9-ui reads from operator, not server
- **Pro Tier**: UI displays insights that operator receives from server
- **Same Architecture**: Operator handles all server communication

## Success Metrics

### Adoption
- 1,000+ Helm chart installations in first year
- Active community contributions and PRs
- Positive user feedback and reviews
- Growing user base among non-VS Code users

### Feature Completeness
- Feature parity with kube9-vscode for free tier
- Smooth operator integration
- Reliable performance with large clusters
- Responsive design across devices

### Ecosystem Impact
- Increases kube9 adoption among non-VS Code users
- Complements VS Code extension
- Strengthens kube9 ecosystem
- Drives operator adoption

## Future Possibilities

### Advanced Features
- Multi-cluster federation views
- Custom dashboards and widgets
- Plugin system for extensibility
- Integration with CI/CD pipelines
- Cost optimization visualization

### Enterprise Features
- SSO integration (OAuth, OIDC, SAML)
- Advanced RBAC and permissions
- Audit logging and compliance
- Custom branding and white-labeling
- Team collaboration features

### Platform Expansion
- Mobile app companion
- CLI tool integration
- API for programmatic access
- Integration marketplace
- Community-contributed plugins

## Core Values

1. **User Empowerment**: Give users control over their deployment and security
2. **Open Source**: Community-driven development and transparency
3. **Feature Parity**: Consistent experience across VS Code and web interfaces
4. **Performance**: Fast, responsive UI that doesn't compromise on features
5. **Simplicity**: Easy installation and configuration, complex features made simple

---

**Built with ❤️ by Alto9 - Making Kubernetes management accessible to everyone**

