---
layout: post
title:  "Hashicorp Vault Monitoring"
date:   2024-03-05 08:46:14 +0300
categories: jekyll update
---

By design after sealing your Vault the monitoring metrics become unavailable("vault.core.unsealed" for example).
To solve this problem we need to add the following configuration:
"""
listener "tcp" {
  address       = "x.x.x.x:8200"
  telemetry {
    unauthenticated_metrics_access = true
  }
}
"""

NOTE: We are setting "unauthenticated_metrics_access" to "true". This is giving us the oportunity to have metrics whilst Vault is sealed.


