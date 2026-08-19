# Project Recommendations

Reviewed: 2026-08-19

## Decision

Keep as a Docker-based Ansible learning tutorial. The K3s homelab content has been
moved to a separate repository: `ansible-homelab/`.

## Value

- Clean, reproducible Docker-based Ansible tutorial.
- Uses inventory.ini, Debian containers, and SSH keys for hands-on learning.
- Serves a different purpose than the K3s homelab work.

## Priority Work

1. Fix Docker container health check (currently references non-existent sshd -T output).
2. Pin the Debian base image version for reproducibility.
3. Consider removing hardcoded SSH passwords from docker-compose.yml for a security-first example.

## Publication Position

Public tutorial reference. Keep it simple and working for Ansible beginners.
