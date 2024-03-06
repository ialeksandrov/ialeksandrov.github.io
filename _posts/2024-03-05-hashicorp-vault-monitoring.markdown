---
layout: post
title:  "Hashicorp Vault Monitoring"
date:   2024-03-05 08:46:14 +0300
categories: jekyll update
---
We are enabling the metrics with the following configuration:
``` 
telemetry {
    disable_hostname = true
    prometheus_retention_time = "12h"
}
```

By design after sealing your Vault the monitoring metrics become unavailable("vault.core.unsealed" for example).
To solve this problem we need to add the following configuration:
```
listener "tcp" {
  address       = "x.x.x.x:8200"
  telemetry {
    unauthenticated_metrics_access = true
  }
}
```
NOTE: We are setting "unauthenticated_metrics_access" to "true". This allow us to retrieve metrics even when the Vault is sealed. We can use the metrics to create alarms.
