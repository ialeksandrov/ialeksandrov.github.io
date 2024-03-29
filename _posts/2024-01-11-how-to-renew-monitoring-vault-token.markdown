---
layout: post
title:  "How to renew automatically the monitoring vault token of Prometheus"
date:   2024-01-11 22:21:14 +0300
categories: jekyll update
---

You probably had the problem to have an expired token for your monitoring in the most inappropriate moment (your
 vacation for example). The default ttl for this Prometheus token in most times is 32 days. There a lot of ways to solve
this issue. I managed to solve it in the following way. We are creating an app role for prometheus-metrics:
```
$ vault write auth/approle/role/prometheus-metrics token_ttl=25h token_max_ttl=168h secret_id_ttl=0 token_policies=prometheus-metrics
```
The important option here is <b>token_ttl=25h</b>.
We are extracting the <b>role_id</b> and <b>secret_id</b>:
```
$ vault read -format=json auth/approle/role/prometheus-metrics/role-id |jq -r .data.role_id
```
```
$ vault write -f -format=json auth/approle/role/prometheus-metrics/secret-id |jq -r .data.secret_id
```
After we have the output from these two commands we can proceed to renewing the token.\
We are doing it with the following command:
```
$ curl -X POST -d '{"role_id":"<role_id>", "secret_id":"secret_id"}' https://<vault-host>/v1/auth/approle/login -sS | jq -r '.auth.client_token' > /etc/prometheus/prometheus-token
```
To automate these manual step we can put this curl query in a shell script which will be executed by a cron job on every
24 hours. This way you skip the manual token renewal every 32 days.

NOTE: These steps can work if you have defined your prometheus job for vault monitoring in the following way:
```
scrape_configs:
  - job_name: vault
    metrics_path: /v1/sys/metrics
    params:
      format: ['prometheus']
    scheme: http
    authorization:
      credentials_file: /etc/prometheus/prometheus-token
    static_configs:
    - targets: ['<vault_ip>:8200']
```
