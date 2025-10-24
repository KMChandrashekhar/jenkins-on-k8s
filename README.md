**Jenkins on Kubernetes Assignment**

---

# 1. Introduction
Objective: Deploy Jenkins as a master pod on Kubernetes, configure dynamic agent pods to run pipeline jobs, and verify execution.
Environment: iximiuz lab Kubernetes cluster using NodePort for web access.

---

# 2. Architecture Diagram
(Optional) Draw a diagram showing:
[Browser] → NodePort → [Jenkins Master Pod] → Kubernetes Cloud → [Agent Pod]

---

# 3. Jenkins Master Deployment
## Deployment YAML Snippet
Highlight key parts: image, ports, ServiceAccount.

## NodePort Service YAML
Exposing port 30000 for web UI and 50000 for agent pods.

---

# 4. ServiceAccount & RBAC
Created `ServiceAccount: jenkins` and `ClusterRoleBinding: jenkins` for permissions.

### Screenshot 3: ServiceAccount & ClusterRoleBinding
`kubectl get sa jenkins`
`kubectl get clusterrolebinding jenkins`

---

# 5. Configuring Jenkins Kubernetes Cloud
Steps:
1. Manage Jenkins → Configure System → Clouds → Add new cloud → Kubernetes
2. Fields:
   - Kubernetes URL: `https://kubernetes.default:443`
   - Jenkins URL: `http://jenkins:8080`
   - Jenkins Tunnel: `jenkins:50000`
   - Namespace: `default`
   - Credentials: Kubernetes Service Account
   - Skip TLS Verify: ✅

### Screenshot 4: Test Connection Success

---

# 6. Adding Pod Template for Agents
- Label: `jenkins-agent`
- Container Name: `jnlp`
- Image: `jenkins/inbound-agent:lts-jdk17`
- Command / Args: empty
- Working Directory: `/home/jenkins/agent`

### Screenshot 5: Pod Template Configuration

---

# 7. Pipeline Job Execution
- Example pipeline: `node { echo "Hello from agent pod" }`
- Trigger Build Now.

### Screenshot 6: Jenkins Console Output

### Screenshot 7: Agent Pod Creation
`kubectl get pods -w`

### Screenshot 8: Agent Pod Logs
`kubectl logs <agent-pod-name>`
```
INFO: Connected
INFO: Jenkins agent is running
```

---

# 8. Conclusion
- Jenkins master running on Kubernetes
- Agent pods spin up dynamically to execute jobs
- Pipeline successfully executed using an agent pod
- NodePort service allowed web UI access

---

# 9. Optional Extras
- Node IP and browser access URL
- Error messages solved (e.g., missing JNLP port, RBAC issues)

