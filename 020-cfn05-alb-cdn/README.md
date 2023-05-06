```mermaid
---
title: CloudFormationの依存関係図
---

flowchart LR
  subgraph storage
    s3-bucket.yaml
  end

  subgraph security, monitoring
    iam-role.yaml
    iam-user.yaml
    cloudtrail.yaml
    secretsmanager.yaml
  end

  subgraph network
    vpc.yaml
    subnet.yaml
    sg.yaml
  end

  subgraph routing, CDN
    route53.yaml
    acm.yaml
    alb.yaml
    cf.yaml
  end

  subnet.yaml -- vpc-id \n igw-id --> vpc.yaml
  sg.yaml -- vpc-id --> vpc.yaml

  cloudtrail.yaml -- cloudtrail-role-arn --> iam-role.yaml

  acm.yaml -. HostedZoneId .-> route53.yaml

  %% prefix
  alb.yaml -- aws-resource-access-logs-s3-bucket-name --> s3-bucket.yaml
  alb.yaml -- acm-arn --> acm.yaml
  alb.yaml -. Subnet::Id .-> subnet.yaml
  alb.yaml -. SecurityGroup::Id .-> sg.yaml
  alb.yaml -. cf-custom-header .-> secretsmanager.yaml

  cf.yaml -- alb-dns --> alb.yaml
  cf.yaml -- aws-resource-access-logs-s3-bucket-domain-name --> s3-bucket.yaml
  cf.yaml -- public-hosted-zone-id --> route53.yaml
  cf.yaml -. ACM .-> acm.yaml
  cf.yaml -. cf-custom-header .-> secretsmanager.yaml
```
