<div class="axi-header">
  <h1>Ingest using Elastic Beats</h1>
</div>


[Elasticbeats](https://www.elastic.co/beats/) serves as a lightweight platform for data shippers that transfer information from the source to axiom and other tools based on the configuration. Before shipping data, beats collects metrics and logs from different sources, which later are deployed to your Axiom instance/deployments.

There are different [Elastic Beats](https://www.elastic.co/beats/) you could use to ship logs and Axioms documentation provides a detailed step by step procedure on how to use each Beats.


## Filebeat

[Filebeat](https://www.elastic.co/beats/filebeat) is a lightweight shipper for logs, it helps you centralize logs, files and can read files from your system.

Filebeats is useful for workloads, system, application log files, and data logs you would like to ingest to Axiom in some way.

In the logging case, it helps centralize logs and files in a structured pattern by reading from your various applications, services, workloads and VMs, then shipping to your Axiom instance/deployments.

### **Installation**

Visit the [Filebeat OSS download page](https://www.elastic.co/downloads/beats/filebeat-oss) to install Filebeat. For more information, check out Filebeat's [official documentation](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)

**When downloading Filebeats, install the OSS version being that the non-oss version doesn't work with Axiom**

### **Configuration**

Axiom lets you ingest data with the ElasticSearch bulk ingest API.

In order for Filebeat to work, index lifecycle management (ILM) must be disabled. To do so, **add setup.ilm.enabled: false** to the filebeat.yml configuration file.

```yaml
setup.ilm.enabled: false
filebeat.inputs:
  - type: log
    # Specify the path of the system log files to be sent to Axiom deployment. 
    paths: 
      - $PATH_TO_LOG_FILE
output.elasticsearch:
  # using the specified Axiom API
  hosts: ["$YOUR_AXIOM_URL:443/api/v1/datasets/<my-datasets>/elastic"]
  # api_key can be your ingest or personal token
  api_key: "user:token"
```

## Metricbeat

[Metricbeat](https://www.elastic.co/beats/metricbeat) is a lightweight shipper for metrics.

Metricbeat is installed on your systems and services and used for monitoring their performance, as well as different remote packages/utilities running on them.

### **Installation**

Visit the [MetricBeat OSS download page](https://www.elastic.co/downloads/beats/metricbeat-oss) to install Metricbeat. For more information, check out Metricbeat's [official documentation](https://www.elastic.co/guide/en/beats/metricbeat/current/index.html)

### **Configuration**

```yaml
metricbeat.modules:
setup.ilm.enabled: false
metricbeat.config.modules:
  path:
    -$PATH_TO_LOG_FILE 
metricbeat.modules:
- module: system 
  metricsets: 
    - filesystem
    - cpu
    - load
    - fsstat
    - memory
    - network
output.elasticsearch:
  hosts: ["$YOUR_AXIOM_URL:443/api/v1/datasets/<dataset>/elastic"]
  # api_key can be your ingest or personal token
  api_key:  "user:token"
```
