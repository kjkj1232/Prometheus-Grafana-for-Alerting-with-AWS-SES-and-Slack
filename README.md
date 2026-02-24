# Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack

* “This project is for practice and documentation purposes, and the EC2 and IAM configurations have been suspended.”




## Objective

Monitoring of AWS EC2 instances.
Visual dashboards powered by Grafana.
Alert notifications via Slack and AWS SES SMTP.



## build relies on:
* [`AWS EC2`](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) - Amazon EC2 provides on-demand, scalable computing capacity in the Amazon Web Services (AWS) Cloud
* [`Prometheus`](https://prometheus.io/) - A system with a dimensional data model, flexible query language, efficient time series database.
* [`Grafana`](https://github.com/grafana/grafana) - analytics & monitoring tool that plugs with many sources, including prometheus.
* [`node_exporter`](https://github.com/prometheus/node_exporter) - Node exporter is the way to collect the Linux server related metrics and statistics for monitoring.
* [`Alert Manager`](https://github.com/prometheus/alertmanager) - analytics & monitoring tool that plugs with many sources, including prometheus.


## Alertmanager yml setting
```
global:
  resolve_timeout: 1m
  slack_api_url: '$slack_api_url'

route:
  receiver: 'slack-email'

receivers:
- name: 'slack-email'
  email_configs:
      - to: 'XXX.com'
        from: 'xxx.com'
        smarthost: 'email-smtp.us-west-2.amazonaws.com:587'
        auth_username: '$SMTP_user_name'
        auth_password: '$SMTP_password'

        send_resolved: false

  slack_configs:
  - channel: '#prometheus'
    send_resolved: false
```

## Prometheus Configurations
```
global:
  scrape_interval: 15s 
  evaluation_interval: 15s 

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
           - localhost:9093

rule_files:
  - "cpu_thresholds_rules.yml"
  - "storage_thresholds_rules.yml"
  - "memory_thresholds_rules.yml"
  - "instance_shutdown_rules.yml"

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    scrape_interval: 5s
    static_configs:
      - targets: ['ec1node:9100',ec2node:9100]
```


## Example Screenshot
Grafana
![image](https://github.com/kjkj1232/Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack/blob/main/Grafana.png)


AWS EC2 
![image](https://github.com/kjkj1232/Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack/blob/main/EC2.png)

Security Groups
![image](https://github.com/kjkj1232/Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack/blob/main/Security%20Groups.png)

prometheus Alerts
![image](https://github.com/kjkj1232/Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack/blob/main/prometheus.png)

Slack Alert 
![image](https://github.com/kjkj1232/Prometheus-Grafana-and-Alerting-with-AWS-SES-with-Slack/blob/main/Slack.png)


# TO DO LIST
Architecture diagram
VPC Setting and ALB Optimization
Prometheus Rules Alert Optimization
