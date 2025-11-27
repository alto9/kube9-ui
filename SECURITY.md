# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 0.x     | :white_check_mark: |

## Reporting a Vulnerability

We take security vulnerabilities seriously. If you discover a security issue, please follow these steps:

### Do NOT:
- Open a public GitHub issue
- Discuss the vulnerability publicly
- Share details on social media or forums

### Do:
1. **Email security@alto9.com** with details
2. Include the following information:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)
   - Affected versions

### What to Expect:
- **Acknowledgment** within 48 hours
- **Regular updates** on progress (at least weekly)
- **Credit** in release notes (if desired)
- **Coordination** on disclosure timing

### Disclosure Timeline:
- We aim to address **critical vulnerabilities** within 7 days
- We aim to address **high severity** vulnerabilities within 30 days
- We will coordinate disclosure timing with you
- We follow **responsible disclosure** practices

## Security Best Practices

When deploying kube9-ui:

### Installation
- **Private by Default**: kube9-ui is not exposed externally by default
- **Port-Forward Access**: Use `kubectl port-forward` for local access
- **Review RBAC**: Check ServiceAccount permissions before installation

### Public Exposure (If Needed)
- **Add Authentication**: kube9-ui has no built-in auth - add your own (OAuth, OIDC, etc.)
- **Enable TLS**: Configure HTTPS via Ingress or LoadBalancer
- **Network Policies**: Consider NetworkPolicy to restrict access
- **Audit Access**: Monitor who accesses the UI

### Configuration
- **Least Privilege**: Use minimal RBAC permissions
- **Resource Limits**: Set appropriate CPU/memory limits
- **Namespace Isolation**: Consider dedicated namespace

## Known Security Considerations

### Authentication
- **No Built-in Auth**: kube9-ui does not provide authentication
- **User Responsibility**: Add your own auth layer (OAuth, OIDC, basic auth)
- **Like ArgoCD**: Security model follows ArgoCD pattern

### RBAC Permissions
kube9-ui requires:
- **Read OperatorStatus CRD** (when operator present)
- **Read/Write Kubernetes resources** (for cluster management)
- **Watch resources** (for real-time updates)

Review the RBAC manifests in the Helm chart before installation.

### Network Security
- **Private by Default**: ClusterIP service, not exposed externally
- **User-Controlled Exposure**: You decide if/how to expose publicly
- **TLS Configuration**: Add TLS via Ingress annotations

### Container Security
- **Non-root**: Runs as non-root user
- **Read-only filesystem**: Container filesystem is read-only where possible
- **Minimal base image**: Uses minimal Node.js base image

## Security Updates

Security updates will be:
- Released as patch versions (e.g., 0.1.1)
- Documented in CHANGELOG.md
- Announced via GitHub releases
- Tagged with security label

## Questions?

For security-related questions (not vulnerabilities), please use:
- GitHub Discussions (public questions)
- GitHub Issues (non-sensitive questions)

For vulnerabilities, always use: **security@alto9.com**

---

**Thank you for helping keep kube9-ui secure!**


