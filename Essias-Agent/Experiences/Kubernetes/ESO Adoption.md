# ESO Adoption

## Context

Secret rotation was manual and required operational intervention.

## Problem

Teams frequently forgot to rotate credentials.

## Analysis

Evaluated:

- External Secrets Operator
- CSI Driver
- Vault
- Native Kubernetes Secrets

## Decision

Adopt External Secrets Operator integrated with Secret Manager.

## Why

- Simpler operation
- Lower maintenance cost
- Native cloud integration

## Result

- Reduced manual work
- Improved security posture
- Easier auditing

## Lessons Learned

ClusterSecretStore simplifies management in multi-namespace environments.