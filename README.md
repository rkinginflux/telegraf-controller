# telegraf-controller config
specific for telegraf daemonset in Kubernetes

```toml
[[outputs.heartbeat]]
url = "https://telegraf-controller01.lab.tld/agents/heartbeat"
instance_id = "$HOSTNAME"
interval = "1m"
include = ["hostname", "statistics", "configs", "status"]
# Include to authorize with Telegraf Controller
# Note: If using Telegraf 1.38.4 or earlier, use INFLUX_TOKEN
token = "${TELEGRAF_CONTROLLER_TOKEN}"

[outputs.heartbeat.status]
default = "ok"

[global_tags]
  cluster_id = "202d986a-3005-46ff-a147-a4f64bcf2e04"
  customer = "Rick's Home Lab"
  country = "USA"
  state = "Idaho"
  region = "us-west-1"

[agent]
  collection_jitter = "0s"
  debug = false
  flush_interval = "10s"
  flush_jitter = "0s"
  hostname = "$HOSTNAME"
  interval = "10s"
  logfile = ""
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  omit_hostname = false
  precision = ""
  quiet = false
  round_interval = true

[[processors.enum]]
  [[processors.enum.mapping]]
    dest = "status_code"
    field = "status"
    [processors.enum.mapping.value_mappings]
        critical = 3
        healthy = 1
        problem = 2

[[outputs.influxdb_v2]]
  bucket = "telegraf"
  timeout = "30s"
  token = "${TOKEN}"
  urls = [ "http://db3-influxdb3-enterprise-ingester.influxdb3.svc.cluster.local:8181" ]
  

#[[inputs.github]]
#  repositories = ["influxdata/EAR"]
#  access_token = "${GITHUB_TOKEN}"

#[[inputs.internet_speed]]
#  interval = "15m"
#  memory_saving_mode = true
  
#[[inputs.cgroup]]
[[inputs.diskio]]
[[inputs.kernel]]
[[inputs.mem]]
[[inputs.net]]
#[[inputs.netstat]]
[[inputs.syslog]]
[[inputs.processes]]
[[inputs.system]]
#[[inputs.prometheus]]
#  timeout = "30s"
#  bearer_token_string = "${TOKEN}"
#  urls = [ "http://db3-influxdb3-enterprise-ingester.influxdb3.svc.cluster.local:8181/metrics" ]

[[inputs.kube_inventory]]
  namespace = ""
  bearer_token = "/var/run/secrets/kubernetes.io/serviceaccount/token"
  insecure_skip_verify = true
  url = "https://kubernetes.default.svc"
  [inputs.kube_inventory.tags]
    cluster = "idaho"

[[inputs.cpu]]
  collect_cpu_time = false
  percpu = true
  report_active = false
  totalcpu = true

[[inputs.disk]]
  ignore_fs = [ "tmpfs", "devtmpfs", "devfs", "iso9660", "overlay", "aufs", "squashfs" ]

[[inputs.statsd]]
  allowed_pending_messages = 10000
  metric_separator = "_"
  percentile_limit = 1000
  percentiles = [ 50.0, 95.0, 99.0 ]
  service_address = ":8125"

[[inputs.internal]]
  collect_memstats = true
```

