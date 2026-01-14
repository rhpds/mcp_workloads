# ocp4_workload_mcp_with_openshift_validation

Validation role for the MCP with OpenShift workshop.

## Overview

This role performs comprehensive health checks on deployed workshop components, including:

- **Shared Components**: Gitea, OpenShift GitOps, OpenShift Pipelines, Toolhive
- **Per-User Components**: LibreChat, MCP OpenShift Server, MCP Gitea Server, Agent, Showroom
- **GitOps Status**: Argo CD application sync and health status

## Features

- Auto-discovery of namespaces using pattern matching
- Pod health and readiness checks
- HTTP endpoint accessibility validation
- GitOps sync status verification
- Detailed health reporting with per-component status
- Integration with agnosticd_user_info for catalog display

## Health Status Levels

- **HEALTHY**: All components running and accessible
- **DEGRADED**: Some components have partial issues (not all pods ready, HTTP not accessible)
- **FAILED**: Critical components missing or not running

## Role Variables

See `defaults/main.yml` for all configurable variables.

Key variables:

```yaml
ocp4_workload_mcp_with_openshift_validation_enabled: true
ocp4_workload_mcp_with_openshift_validation_num_users: 1
ocp4_workload_mcp_with_openshift_validation_user_prefix: "user"

# Feature flags for component checks
ocp4_workload_mcp_with_openshift_validation_check_librechat: true
ocp4_workload_mcp_with_openshift_validation_check_argocd_apps: true
# ... etc
```

## Dependencies

- `kubernetes.core` collection
- `agnosticd.core` collection (for agnosticd_user_info)

## Example Playbook

```yaml
- name: Validate MCP with OpenShift Workshop
  hosts: localhost
  connection: local
  gather_facts: false

  tasks:
    - name: Run validation role
      ansible.builtin.include_role:
        name: rhpds.mcp_workloads.ocp4_workload_mcp_with_openshift_validation
      vars:
        ocp4_workload_mcp_with_openshift_validation_num_users: 3
```

## Testing

To test the validation role manually:

```bash
ansible-playbook playbooks/validate_mcp_with_openshift.yml
```

## Author

Prakhar Srivastava - Manager, Technical Marketing at Red Hat
