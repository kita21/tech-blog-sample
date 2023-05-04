```mermaid
---
title: CloudFormationの依存関係図
---

flowchart LR
  subgraph security, monitoring
    direction LR
    iam-role.yaml
    iam-user.yaml
    cloudtrail.yaml
  end

  cloudtrail.yaml -- cloudtrail-role-arn --> iam-role.yaml
```
