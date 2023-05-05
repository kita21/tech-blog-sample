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

  subgraph network
    direction LR
    vpc.yaml
    subnet.yaml
    sg.yaml
  end

  subgraph routing, CDN
    direction LR
    route53.yaml
    acm.yaml
  end

  subnet.yaml -- vpc-id \n igw-id --> vpc.yaml
  sg.yaml -- vpc-id --> vpc.yaml

  cloudtrail.yaml -- cloudtrail-role-arn --> iam-role.yaml

  acm.yaml -. HostedZoneId .-> route53.yaml
```
