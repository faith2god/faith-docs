---
date: 2023-12-20
authors: [fuyuanyuan]
title: k8s-metrics
description: >
  k8s-metrics
keywords: [metrics, kube-state-metrics, cadvisor]
categories:
  - prometheus
tags: [metrics, kube-state-metrics, cadvisor]
---

## K8S监控指标

### kube-state-metrics
官方文档 https://github.com/kubernetes/kube-state-metrics/blob/main/docs/README.md

#### pod-metrics

| Metric name                                           | Metric type | Description                                                                                                                                                                         | Unit (where applicable)                        | Labels/tags                                                                                                                                                                                                                                                                                                                                                       | Status       | Opt-in |
| ----------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------ |
| kube_pod_annotations                                  | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [--metric-annotations-allowlist](./cli-arguments.md)                                                           |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `annotation_POD_ANNOTATION`=&lt;POD_ANNOTATION&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                  | EXPERIMENTAL | -      |
| kube_pod_info                                         | Gauge       | Information about pod                                                                                                                                                               |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `host_ip`=&lt;host-ip&gt; <br> `pod_ip`=&lt;pod-ip&gt; <br> `node`=&lt;node-name&gt;<br> `created_by_kind`=&lt;created_by_kind&gt;<br> `created_by_name`=&lt;created_by_name&gt;<br> `uid`=&lt;pod-uid&gt;<br> `priority_class`=&lt;priority_class&gt;<br> `host_network`=&lt;host_network&gt; | STABLE       | -      |
| kube_pod_ips                                          | Gauge       | Pod IP addresses                                                                                                                                                                    |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `ip`=&lt;pod-ip-address&gt; <br> `ip_family`=&lt;4 OR 6&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                         | EXPERIMENTAL | -      |
| kube_pod_start_time                                   | Gauge       | Start time in unix timestamp for a pod                                                                                                                                              | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | STABLE       | -      |
| kube_pod_completion_time                              | Gauge       | Completion time in unix timestamp for a pod                                                                                                                                         | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | STABLE       | -      |
| kube_pod_owner                                        | Gauge       | Information about the Pod's owner                                                                                                                                                   |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `owner_kind`=&lt;owner kind&gt; <br> `owner_name`=&lt;owner name&gt; <br> `owner_is_controller`=&lt;whether owner is controller&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                 | STABLE       | -      |
| kube_pod_labels                                       | Gauge       | Kubernetes labels converted to Prometheus labels controlled via [--metric-labels-allowlist](./cli-arguments.md)                                                                     |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `label_POD_LABEL`=&lt;POD_LABEL&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                 | STABLE       | -      |
| kube_pod_nodeselectors                                | Gauge       | Describes the Pod nodeSelectors                                                                                                                                                     |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `nodeselector_NODE_SELECTOR`=&lt;NODE_SELECTOR&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                  | EXPERIMENTAL | Opt-in |
| kube_pod_status_phase                                 | Gauge       | The pods current phase                                                                                                                                                              |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `phase`=&lt;Pending\|Running\|Succeeded\|Failed\|Unknown&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                        | STABLE       | -      |
| kube_pod_status_qos_class                             | Gauge       | The pods current qosClass                                                                                                                                                           |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `qos_class`=&lt;BestEffort\|Burstable\|Guaranteed&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                               | EXPERIMENTAL | -      |
| kube_pod_status_ready                                 | Gauge       | Describes whether the pod is ready to serve requests                                                                                                                                |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `condition`=&lt;true\|false\|unknown&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                            | STABLE       | -      |
| kube_pod_status_scheduled                             | Gauge       | Describes the status of the scheduling process for the pod                                                                                                                          |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `condition`=&lt;true\|false\|unknown&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                            | STABLE       | -      |
| kube_pod_container_info                               | Gauge       | Information about a container in a pod                                                                                                                                              |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `image`=&lt;image-name&gt; <br> `image_id`=&lt;image-id&gt; <br> `image_spec`=&lt;image-spec&gt; <br> `container_id`=&lt;containerid&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                    | STABLE       | -      |
| kube_pod_container_status_waiting                     | Gauge       | Describes whether the container is currently in waiting state                                                                                                                       |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_container_status_waiting_reason              | Gauge       | Describes the reason the container is currently in waiting state                                                                                                                    |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;container-waiting-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                   | STABLE       | -      |
| kube_pod_container_status_running                     | Gauge       | Describes whether the container is currently in running state                                                                                                                       |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_container_state_started                      | Gauge       | Start time in unix timestamp for a pod container                                                                                                                                    | seconds                                        | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_container_status_terminated                  | Gauge       | Describes whether the container is currently in terminated state                                                                                                                    |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_container_status_terminated_reason           | Gauge       | Describes the reason the container is currently in terminated state                                                                                                                 |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;container-terminated-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                | EXPERIMENTAL | -      |
| kube_pod_container_status_last_terminated_reason      | Gauge       | Describes the last reason the container was in terminated state                                                                                                                     |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;last-terminated-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                     | EXPERIMENTAL | -      |
| kube_pod_container_status_last_terminated_exitcode    | Gauge       | Describes the exit code for the last container in terminated state.                                                                                                                 |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | EXPERIMENTAL | -      |
| kube_pod_container_status_ready                       | Gauge       | Describes whether the containers readiness check succeeded                                                                                                                          |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_status_initialized_time                      | Gauge       | Time when the pod is initialized.                                                                                                                                                   | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL |
| kube_pod_status_ready_time                            | Gauge       | Time when pod passed readiness probes.                                                                                                                                              | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL |
| kube_pod_status_container_ready_time                  | Gauge       | Time when the container of the pod entered Ready state.                                                                                                                             | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL |
| kube_pod_container_status_restarts_total              | Counter     | The number of container restarts per container                                                                                                                                      |                                                | `container`=&lt;container-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `pod`=&lt;pod-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_container_resource_requests                  | Gauge       | The number of requested request resource by a container. It is recommended to use the `kube_pod_resource_requests` metric exposed by kube-scheduler instead, as it is more precise. | `cpu`=&lt;core&gt; <br> `memory`=&lt;bytes&gt; | `resource`=&lt;resource-name&gt; <br> `unit`=&lt;resource-unit&gt; <br> `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `node`=&lt; node-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                           | EXPERIMENTAL | -      |
| kube_pod_container_resource_limits                    | Gauge       | The number of requested limit resource by a container. It is recommended to use the `kube_pod_resource_limits` metric exposed by kube-scheduler instead, as it is more precise.     | `cpu`=&lt;core&gt; <br> `memory`=&lt;bytes&gt; | `resource`=&lt;resource-name&gt; <br> `unit`=&lt;resource-unit&gt; <br> `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `node`=&lt; node-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                           | EXPERIMENTAL | -      |
| kube_pod_overhead_cpu_cores                           | Gauge       | The pod overhead in regards to cpu cores associated with running a pod                                                                                                              | core                                           | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL | -      |
| kube_pod_overhead_memory_bytes                        | Gauge       | The pod overhead in regards to memory associated with running a pod                                                                                                                 | bytes                                          | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL | -      |
| kube_pod_runtimeclass_name_info                       | Gauge       | The runtimeclass associated with the pod                                                                                                                                            |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL | -      |
| kube_pod_created                                      | Gauge       | Unix creation timestamp                                                                                                                                                             | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | STABLE       | -      |
| kube_pod_deletion_timestamp                           | Gauge       | Unix deletion timestamp                                                                                                                                                             | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | EXPERIMENTAL | -      |
| kube_pod_restart_policy                               | Gauge       | Describes the restart policy in use by this pod                                                                                                                                     |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `type`=&lt;Always\|Never\|OnFailure&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                             | STABLE       | -      |
| kube_pod_init_container_info                          | Gauge       | Information about an init container in a pod                                                                                                                                        |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `image`=&lt;image-name&gt; <br> `image_id`=&lt;image-id&gt; <br> `image_spec`=&lt;image-spec&gt; <br> `container_id`=&lt;containerid&gt; <br> `uid`=&lt;pod-uid&gt; <br> `restart_policy`=&lt;restart-policy&gt;                                                                                   | STABLE       | -      |
| kube_pod_init_container_status_waiting                | Gauge       | Describes whether the init container is currently in waiting state                                                                                                                  |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_init_container_status_waiting_reason         | Gauge       | Describes the reason the init container is currently in waiting state                                                                                                               |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;container-waiting-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                   | EXPERIMENTAL | -      |
| kube_pod_init_container_status_running                | Gauge       | Describes whether the init container is currently in running state                                                                                                                  |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_init_container_status_terminated             | Gauge       | Describes whether the init container is currently in terminated state                                                                                                               |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_init_container_status_terminated_reason      | Gauge       | Describes the reason the init container is currently in terminated state                                                                                                            |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;container-terminated-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                | EXPERIMENTAL | -      |
| kube_pod_init_container_status_last_terminated_reason | Gauge       | Describes the last reason the init container was in terminated state                                                                                                                |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;last-terminated-reason&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                     | EXPERIMENTAL | -      |
| kube_pod_init_container_status_ready                  | Gauge       | Describes whether the init containers readiness check succeeded                                                                                                                     |                                                | `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_init_container_status_restarts_total         | Counter     | The number of restarts for the init container                                                                                                                                       | integer                                        | `container`=&lt;container-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `pod`=&lt;pod-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                  | STABLE       | -      |
| kube_pod_init_container_resource_limits               | Gauge       | The number of CPU cores requested limit by an init container                                                                                                                        | `cpu`=&lt;core&gt; <br> `memory`=&lt;bytes&gt; | `resource`=&lt;resource-name&gt; <br> `unit`=&lt;resource-unit&gt; <br> `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `node`=&lt; node-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                           | EXPERIMENTAL | -      |
| kube_pod_init_container_resource_requests             | Gauge       | The number of CPU cores requested by an init container                                                                                                                              | `cpu`=&lt;core&gt; <br> `memory`=&lt;bytes&gt; | `resource`=&lt;resource-name&gt; <br> `unit`=&lt;resource-unit&gt; <br> `container`=&lt;container-name&gt; <br> `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `node`=&lt; node-name&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                           | EXPERIMENTAL | -      |
| kube_pod_spec_volumes_persistentvolumeclaims_info     | Gauge       | Information about persistentvolumeclaim volumes in a pod                                                                                                                            |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `volume`=&lt;volume-name&gt;  <br> `persistentvolumeclaim`=&lt;persistentvolumeclaim-claimname&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                  | STABLE       | -      |
| kube_pod_spec_volumes_persistentvolumeclaims_readonly | Gauge       | Describes whether a persistentvolumeclaim is mounted read only                                                                                                                      | bool                                           | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt;  <br> `volume`=&lt;volume-name&gt;  <br> `persistentvolumeclaim`=&lt;persistentvolumeclaim-claimname&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                 | STABLE       | -      |
| kube_pod_status_reason                                | Gauge       | The pod status reasons                                                                                                                                                              |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `reason`=&lt;Evicted\|NodeAffinity\|NodeLost\|Shutdown\|UnexpectedAdmissionError&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                | EXPERIMENTAL | -      |
| kube_pod_status_scheduled_time                        | Gauge       | Unix timestamp when pod moved into scheduled status                                                                                                                                 | seconds                                        | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | STABLE       | -      |
| kube_pod_status_unschedulable                         | Gauge       | Describes the unschedulable status for the pod                                                                                                                                      |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt;                                                                                                                                                                                                                                                                          | STABLE       | -      |
| kube_pod_tolerations                                  | Gauge       | Information about the pod tolerations                                                                                                                                               |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt; <br> `key`=&lt;toleration-key&gt; <br> `operator`=&lt;toleration-operator&gt; <br> `value`=&lt;toleration-value&gt; <br> `effect`=&lt;toleration-effect&gt; `toleration_seconds`=&lt;toleration-seconds&gt;                                                              | EXPERIMENTAL | -      |
| kube_pod_service_account                              | Gauge       | The service account for a pod                                                                                                                                                       |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt; <br> `service_account`=&lt;service_account&gt;                                                                                                                                                                                                                           | EXPERIMENTAL | -      |
| kube_pod_scheduler                              | Gauge       | The scheduler for a pod                                                                                                                                                       |                                                | `pod`=&lt;pod-name&gt; <br> `namespace`=&lt;pod-namespace&gt; <br> `uid`=&lt;pod-uid&gt; <br> `name`=&lt;scheduler-name&gt;                                                                                                                                                                                                                           | EXPERIMENTAL | -      |

##### 使用场景
- 获取pod在 `Unknown`状态
```
sum(kube_pod_status_phase{phase="Unknown"}) by (namespace, pod) or (count(kube_pod_deletion_timestamp) by (namespace, pod) * sum(kube_pod_status_reason{reason="NodeLost"}) by(namespace, pod)) > 0
```
- 获取pod在 `Terminating`状态
```
count(kube_pod_deletion_timestamp) by (namespace, pod) * count(kube_pod_status_reason{reason="NodeLost"} == 0) by (namespace, pod) > 0
```

#### deployment-metrics

### cAdvisor
官网：https://github.com/google/cadvisor/blob/master/docs/storage/prometheus.md#prometheus-container-metrics



## K8S监控语句
### k8s负载
#### KubePodCrashLooping
```
rate(kube_pod_container_status_restarts_total{job="kube-state-metrics"}[5m]) * 60 * 5 > 0
```
或者
```
changes(kube_pod_container_status_restarts_total[5m])> 0
```
报警信息
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}/容器 {{ $labels.container}} 最近5分钟重启{{ printf "%.2f" $value }}次

####  KubePodNotReady
```
sum by (cluster,namespace, pod) (
  max by(cluster,namespace, pod) (
    kube_pod_status_phase{job="kube-state-metrics", phase=~"Pending|Unknown"}
  ) * on(cluster,namespace, pod) group_left(owner_kind) topk by(cluster,namespace, pod) (
    1, max by(cluster,namespace, pod, owner_kind) (kube_pod_owner{owner_kind!="Job"})
  )
) > 0
```
在Kubernetes环境中检查是否存在处于"Pending"或"Unknown"状态的Pod，且这些Pod的所有者（owner）不是"Job"。

- sum by (cluster, namespace, pod): 首先对查询结果进行聚合，按照cluster、namespace和pod这三个标签进行分组。
- max by(cluster, namespace, pod): 在内部，对于每个cluster、namespace和pod的组合，找到满足条件的Pod中最大的值。
- 这里使用的是kube_pod_status_phase{job="kube-state-metrics", phase=~"Pending|Unknown"}查询，它会返回处于"Pending"或"Unknown"状态的Pod。
- on(cluster, namespace, pod) group_left(owner_kind): 使用on关键字指定了需要与前面的结果进行关联的标签，这里是cluster、namespace和pod。
- group_left确保保留左侧查询（即前面的查询）中的标签，并与右侧查询（即后面的查询）进行关联。
- topk by(cluster, namespace, pod):对于每个cluster、namespace和pod的组合，找到满足条件的Pod的所有者，并只选择一个。
topk函数用于选择满足条件的最顶部的记录。这里选择的是前一个查询中的Pod的所有者。
- 1, max by(cluster, namespace, pod, owner_kind) (kube_pod_owner{owner_kind!="Job"}): 这是一个子查询，它查找满足条件的Pod的所有者，并确保这些所有者不是"Job"。
使用max by确保对于每个cluster、namespace、pod和owner_kind的组合，找到最大的值（在这里，我们只是查找任意一个满足条件的记录）。
- `> 0`: 最后，主查询确保结果大于0。这意味着如果找到任何满足条件的Pod，且它的所有者不是"Job"，则结果为真。
综上所述，这段查询语句用于检查Kubernetes集群中是否存在处于"Pending"或"Unknown"状态的Pod，并且这些Pod的所有者不是"Job"。如果存在这样的Pod，查询结果将为真。

报警信息
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}处于NotReady状态超过15分钟

#### KubeDeploymentGenerationMismatch
出现部署版本不匹配
```
kube_deployment_status_observed_generation{job="kube-state-metrics"}
  !=
kube_deployment_metadata_generation{job="kube-state-metrics"}
```
告警信息
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/deployment {{ $labels.deployment}}部署版本不符合预期，表示Deployment变更没有生效

#### KubeDeploymentReplicasMismatch
出现部署副本不匹配。
```
(
  kube_deployment_spec_replicas{job="kube-state-metrics"}
    !=
  kube_deployment_status_replicas_available{job="kube-state-metrics"}
) and (
  changes(kube_deployment_status_replicas_updated{job="kube-state-metrics"}[5m])
    ==
  0
)

```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/deployment {{ $labels.deployment }} 没有达到预期副本数超过5分钟

#### KubeStatefulSetReplicasMismatch
状态集副本不匹配。
```
(
  kube_statefulset_status_replicas_ready{job="kube-state-metrics"}
    !=
  kube_statefulset_status_replicas{job="kube-state-metrics"}
) and (
  changes(kube_statefulset_status_replicas_updated{job="kube-state-metrics"}[5m])
    ==
  0
)
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/statefulset {{ $labels.statefulset }} 没有达到预期副本数超过5分钟

#### KubeStatefulSetGenerationMismatch
状态集版本不匹配。
```
kube_statefulset_status_observed_generation{job="kube-state-metrics"}
  !=
kube_statefulset_metadata_generation{job="kube-state-metrics"}
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/statefulset {{ $labels.statefulset}} 部署版本不符合预期，表示statefulset变更没有生效

#### KubeStatefulSetUpdateNotRolledOut
状态集更新未退出。
```
(
  max without (revision) (
    kube_statefulset_status_current_revision{job="kube-state-metrics"}
      unless
    kube_statefulset_status_update_revision{job="kube-state-metrics"}
  )
    *
  (
    kube_statefulset_replicas{job="kube-state-metrics"}
      !=
    kube_statefulset_status_replicas_updated{job="kube-state-metrics"}
  )
)  and (
  changes(kube_statefulset_status_replicas_updated{job="kube-state-metrics"}[5m])
    ==
  0
)
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/statefulset {{ $labels.statefulset }} 部分Pod未更新
#### KubeDaemonSetRolloutStuck
DaemonSet退出回退。
```
(
  (
    kube_daemonset_status_current_number_scheduled{job="kube-state-metrics"}
     !=
    kube_daemonset_status_desired_number_scheduled{job="kube-state-metrics"}
  ) or (
    kube_daemonset_status_number_misscheduled{job="kube-state-metrics"}
     !=
    0
  ) or (
    kube_daemonset_updated_number_scheduled{job="kube-state-metrics"}
     !=
    kube_daemonset_status_desired_number_scheduled{job="kube-state-metrics"}
  ) or (
    kube_daemonset_status_number_available{job="kube-state-metrics"}
     !=
    kube_daemonset_status_desired_number_scheduled{job="kube-state-metrics"}
  )
) and (
  changes(kube_daemonset_updated_number_scheduled{job="kube-state-metrics"}[5m])
    ==
  0
)
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/daemonset {{ $labels.daemonset }} 变更卡了超过5分钟

#### KubeContainerWaiting
容器等待。
```
sum by (namespace, pod, container,cluster) (kube_pod_container_status_waiting_reason{job="kube-state-metrics"}) > 0
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/pod {{ $labels.pod }}/container {{ $labels.container}} 处于Waiting状态超过10分钟

#### KubeDaemonSetNotScheduled
DaemonSet无计划
```
kube_daemonset_status_desired_number_scheduled{job="kube-state-metrics"}
  -
kube_daemonset_status_current_number_scheduled{job="kube-state-metrics"} > 0
```
告警内容
> 集群 {{ $labels.cluster }}/ namespace {{ $labels.namespace }}/daemonset {{ $labels.daemonset}}  中 {{ $value }} 个 pod 没有被调度

#### KubeDaemonSetMisScheduled
Daemon缺失计划
```
kube_daemonset_status_number_misscheduled{job="kube-state-metrics"} > 0
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/daemonset {{ $labels.daemonset}} 中 {{ $value }} 个 pod 错误调度到node上

#### KubeJobCompletion
任务完成
```
kube_job_spec_completions{job="kube-state-metrics"} - kube_job_status_succeeded{job="kube-state-metrics"}  > 0
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/job {{ $labels.job_name }} 任务完成

#### KubeJobFailed
任务失败
```
kube_job_status_failed{job="kube-state-metrics"} > 0
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/job {{ $labels.job_name }} 执行失败

#### KubeHpaReplicasMismatch
HPA副本不匹配
```
(kube_hpa_status_desired_replicas{job="kube-state-metrics"}
  !=
kube_hpa_status_current_replicas{job="kube-state-metrics"})
  and
changes(kube_hpa_status_current_replicas[10m]) == 0
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/HPA {{ $labels.hpa }} 副本数和预期不一致超过10分钟

#### KubeHpaMaxedOut
HPA副本超过最大值
```
kube_hpa_status_current_replicas{job="kube-state-metrics"}
  ==
kube_hpa_spec_max_replicas{job="kube-state-metrics"}
```
告警内容
> 集群 {{ $labels.cluster }}/namespace {{ $labels.namespace }}/HPA {{ $labels.hpa }} 副本数达到最大值超过5m

### k8s资源
#### KubeCPUOvercommit
CPU过载
```
sum(kube_pod_container_resource_requests_cpu_cores) by (cluster) /sum(kube_node_status_allocatable_cpu_cores) by (cluster)  >
(count(kube_node_status_allocatable_cpu_cores) by (cluster) -1)   / count(kube_node_status_allocatable_cpu_cores) by (cluster) 
```

新版本的Kube_state_metric语句
```
sum(kube_pod_container_resource_requests{resource="cpu"}) by (cluster)  
  /
sum(kube_node_status_allocatable{resource="cpu"})
 by (cluster)  >
(count(kube_node_status_allocatable{resource="cpu"}) by (cluster) -1)   / count(kube_node_status_allocatable{resource="cpu"}) by (cluster) 
```

告警内容
> 集群{{ $labels.cluster }}内Pod申请的CPU过载，当前CPU申请占比{{ $value | humanizePercentage }}


#### KubeMemoryOvercommit
内存过载
```
sum(namespace:kube_pod_container_resource_requests_memory_bytes:sum{}) by (cluster)  
  /
sum(kube_node_status_allocatable_memory_bytes) by (cluster)  
  >
(count(kube_node_status_allocatable_memory_bytes) by (cluster)-1)  
  /
count(kube_node_status_allocatable_memory_bytes) by (cluster)  
```

新版本的Kube_state_metric语句
sum(kube_pod_container_resource_requests{resource="memory"}) by (cluster)  
  /
sum(kube_node_status_allocatable{resource="memory"}) by (cluster)  
  >
(count(kube_node_status_allocatable{resource="memory"}) by (cluster)-1)  
  /
count(kube_node_status_allocatable{resource="memory"}) by (cluster)  

告警内容
> 集群{{ $labels.cluster }}内Pod申请的内存过载，当前CPU申请占比{{ $value | humanizePercentage }}

#### KubeCPUQuotaOvercommit
CPU额度过载（kube_resourcequota不存在，从resourcequota-metrics中 采集）
```
sum(kube_resourcequota{job="kube-state-metrics", type="hard", resource="cpu"})  by (cluster)  
  /
sum(kube_node_status_allocatable_cpu_cores) by (cluster)  
  > 1.5
```
告警内容
> 集群{{ $labels.cluster }}CPU Quota过载，已经超过可分配CPU资源的{{ $value }}倍

#### KubeMemoryQuotaOvercommit
内存额度过载（kube_resourcequota不存在，从resourcequota-metrics中 采集）
```
sum(kube_resourcequota{job="kube-state-metrics", type="hard", resource="memory"}) by (cluster)  
  /
sum(kube_node_status_allocatable_memory_bytes{job="node-exporter"}) by (cluster)   
  > 1.5
```
告警内容
> 集群{{ $labels.cluster }}内存配额过载，已经超过可分配内存资源的{{ $value }}倍

#### KubeQuotaAlmostFull
配额快超过限制了
```
kube_resourcequota{job="kube-state-metrics", type="used"}
  / ignoring(instance, job, type)
(kube_resourcequota{job="kube-state-metrics", type="hard"} > 0)
  > 0.7 
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}中资源{{ $labels.resource }}使用率超过{{ $value | humanizePercentage }}

#### KubeQuotaExceeded
配额超限制了
```
kube_resourcequota{job="kube-state-metrics", type="used"}
  / ignoring(instance, job, type)
(kube_resourcequota{job="kube-state-metrics", type="hard"} > 0)
  > 1
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}中资源{{ $labels.resource }}使用率超过{{ $value | humanizePercentage }}

#### 查询处在RUNNING状态的pod的内存请求量总和，按照namespace、pod和node进行分组
```
sum(kube_pod_container_resource_requests{resource="memory"}) by (namespace, pod, node)
  * on (namespace, pod) group_left() (sum(kube_pod_status_phase{phase="Running"}) by (pod, namespace) == 1)
``` 
- sum(kube_pod_container_resource_requests{resource="memory"}) by (namespace, pod, node): 这部分查询是对每个命名空间（namespace）、Pod和节点（node）的内存请求进行求和。
- kube_pod_container_resource_requests{resource="memory"}这部分是从kube-state-metrics获取的，它会返回每个Pod容器的内存请求。
- on (namespace, pod) group_left(): 使用on关键字指定了需要与前面的结果进行关联的标签，这里是namespace和pod。
group_left()确保保留左侧查询中的标签，并与右侧查询进行关联，但在此例中，右侧查询不需要额外的标签。
- (sum(kube_pod_status_phase{phase="Running"}) by (pod, namespace) == 1): 这是一个子查询，它检查每个Pod是否处于"Running"状态。
使用sum对每个Pod和命名空间的"Running"状态进行求和。由于Pod的状态只能是0或1，所以这个求和的结果要么是0（表示没有Pod处于"Running"状态），要么是1（表示有一个Pod处于"Running"状态）。
- == 1确保只选择处于"Running"状态的Pod。
- 最终，两个查询结果的乘积：这意味着我们希望查找满足两个条件的Pod：一是它们请求了内存资源，二是它们处于"Running"状态。

### CAdvisor
官网文档：https://github.com/google/cadvisor/blob/master/docs/storage/prometheus.md

#### Prometheus container metrics
The table below lists the Prometheus container metrics exposed by cAdvisor (in alphabetical order by metric name) and corresponding `-disable_metrics` / `-enable_metrics` option parameter:

Metric name | Type | Description | Unit (where applicable) | option parameter | additional build flag |
:-----------|:-----|:------------|:------------------------|:---------------------------|:----------------------
`container_blkio_device_usage_total` | Counter | Blkio device bytes usage | bytes | diskIO | 
`container_cpu_cfs_periods_total` | Counter | Number of elapsed enforcement period intervals | | cpu |
`container_cpu_cfs_throttled_periods_total` | Counter | Number of throttled period intervals | | cpu |
`container_cpu_cfs_throttled_seconds_total` | Counter | Total time duration the container has been throttled | seconds | cpu |
`container_cpu_load_average_10s` | Gauge | Value of container cpu load average over the last 10 seconds | | cpuLoad |
`container_cpu_schedstat_run_periods_total` | Counter | Number of times processes of the cgroup have run on the cpu | | sched |
`container_cpu_schedstat_runqueue_seconds_total` | Counter | Time duration processes of the container have been waiting on a runqueue | seconds | sched |
`container_cpu_schedstat_run_seconds_total` | Counter | Time duration the processes of the container have run on the CPU | seconds | sched |
`container_cpu_system_seconds_total` | Counter | Cumulative system cpu time consumed | seconds | cpu |
`container_cpu_usage_seconds_total` | Counter | Cumulative cpu time consumed | seconds | cpu |
`container_cpu_user_seconds_total` | Counter | Cumulative user cpu time consumed | seconds | cpu |
`container_file_descriptors` | Gauge | Number of open file descriptors for the container | | process |
`container_fs_inodes_free` | Gauge | Number of available Inodes | | disk |
`container_fs_inodes_total` | Gauge | Total number of Inodes | | disk |
`container_fs_io_current` | Gauge | Number of I/Os currently in progress | | diskIO |
`container_fs_io_time_seconds_total` | Counter | Cumulative count of seconds spent doing I/Os | seconds | diskIO |
`container_fs_io_time_weighted_seconds_total` | Counter | Cumulative weighted I/O time | seconds | diskIO |
`container_fs_limit_bytes` | Gauge | Number of bytes that can be consumed by the container on this filesystem | bytes | disk |
`container_fs_reads_bytes_total` | Counter | Cumulative count of bytes read | bytes | diskIO |
`container_fs_read_seconds_total` | Counter | Cumulative count of seconds spent reading | | diskIO |
`container_fs_reads_merged_total` | Counter | Cumulative count of reads merged | | diskIO |
`container_fs_reads_total` | Counter | Cumulative count of reads completed | | diskIO |
`container_fs_sector_reads_total` | Counter | Cumulative count of sector reads completed | | diskIO |
`container_fs_sector_writes_total` | Counter | Cumulative count of sector writes completed | | diskIO |
`container_fs_usage_bytes` | Gauge | Number of bytes that are consumed by the container on this filesystem | bytes | disk |
`container_fs_writes_bytes_total` | Counter | Cumulative count of bytes written | bytes | diskIO |
`container_fs_write_seconds_total` | Counter | Cumulative count of seconds spent writing | seconds | diskIO |
`container_fs_writes_merged_total` | Counter | Cumulative count of writes merged | | diskIO |
`container_fs_writes_total` | Counter | Cumulative count of writes completed | | diskIO |
`container_hugetlb_failcnt` | Counter | Number of hugepage usage hits limits | | hugetlb |
`container_hugetlb_max_usage_bytes` | Gauge | Maximum hugepage usages recorded | bytes | hugetlb |
`container_hugetlb_usage_bytes` | Gauge | Current hugepage usage | bytes | hugetlb |
`container_last_seen` | Gauge | Last time a container was seen by the exporter | timestamp | - |
`container_llc_occupancy_bytes` | Gauge | Last level cache usage statistics for container counted with RDT Memory Bandwidth Monitoring (MBM). | bytes | resctrl |
`container_memory_bandwidth_bytes` | Gauge | Total memory bandwidth usage statistics for container counted with RDT Memory Bandwidth Monitoring (MBM). | bytes | resctrl |
`container_memory_bandwidth_local_bytes` | Gauge | Local memory bandwidth usage statistics for container counted with RDT Memory Bandwidth Monitoring (MBM). | bytes | resctrl |
`container_memory_cache` | Gauge | Total page cache memory | bytes | memory |
`container_memory_failcnt` | Counter | Number of memory usage hits limits | | memory |
`container_memory_failures_total` | Counter | Cumulative count of memory allocation failures | | memory |
`container_memory_mapped_file` | Gauge | Size of memory mapped files | bytes | memory |
`container_memory_max_usage_bytes` | Gauge | Maximum memory usage recorded | bytes | memory |
`container_memory_migrate` | Gauge | Memory migrate status | | cpuset |
`container_memory_numa_pages` | Gauge | Number of used pages per NUMA node | | memory_numa |
`container_memory_rss` | Gauge | Size of RSS | bytes | memory |
`container_memory_swap` | Gauge | Container swap usage | bytes | memory |
`container_memory_usage_bytes` | Gauge | Current memory usage, including all memory regardless of when it was accessed | bytes | memory |
`container_memory_working_set_bytes` | Gauge | Current working set | bytes | memory |
`container_network_advance_tcp_stats_total` | Gauge | advanced tcp connections statistic for container | | advtcp |
`container_network_receive_bytes_total` | Counter | Cumulative count of bytes received | bytes | network |
`container_network_receive_errors_total` | Counter | Cumulative count of errors encountered while receiving | | network |
`container_network_receive_packets_dropped_total` | Counter | Cumulative count of packets dropped while receiving | | network |
`container_network_receive_packets_total` | Counter | Cumulative count of packets received | | network |
`container_network_tcp6_usage_total` | Gauge | tcp6 connection usage statistic for container | | tcp |
`container_network_tcp_usage_total` | Gauge | tcp connection usage statistic for container | | tcp |
`container_network_transmit_bytes_total` | Counter | Cumulative count of bytes transmitted | bytes | network |
`container_network_transmit_errors_total` | Counter | Cumulative count of errors encountered while transmitting | | network |
`container_network_transmit_packets_dropped_total` | Counter | Cumulative count of packets dropped while transmitting | | network |
`container_network_transmit_packets_total` | Counter | Cumulative count of packets transmitted | | network |
`container_network_udp6_usage_total` | Gauge | udp6 connection usage statistic for container | | udp |
`container_network_udp_usage_total` | Gauge | udp connection usage statistic for container | | udp |
`container_oom_events_total` | Counter | Count of out of memory events observed for the container | | oom_event |
`container_perf_events_scaling_ratio` | Gauge | Scaling ratio for perf event counter (event can be identified by `event` label and `cpu` indicates the core for which event was measured). See [perf event configuration](../runtime_options.md#perf-events). | | perf_event | libpfm
`container_perf_events_total` | Counter | Scaled counter of perf core event (event can be identified by `event` label and `cpu` indicates the core for which event was measured). See [perf event configuration](../runtime_options.md#perf-events). | | perf_event | libpfm
`container_perf_uncore_events_scaling_ratio` | Gauge | Scaling ratio for perf uncore event counter (event can be identified by `event` label, `pmu` and `socket` lables indicate the PMU and the CPU socket for which event was measured). See [perf event configuration](../runtime_options.md#perf-events). Metric exists only for main cgroup (id="/"). | | perf_event | libpfm
`container_perf_uncore_events_total` | Counter | Scaled counter of perf uncore event (event can be identified by `event` label, `pmu` and `socket` lables indicate the PMU and the CPU socket for which event was measured). See [perf event configuration](../runtime_options.md#perf-events)). Metric exists only for main cgroup (id="/").| | perf_event | libpfm
`container_processes` | Gauge | Number of processes running inside the container | | process |
`container_referenced_bytes` | Gauge |  Container referenced bytes during last measurements cycle based on Referenced field in /proc/smaps file, with /proc/PIDs/clear_refs set to 1 after defined number of cycles configured through `referenced_reset_interval` cAdvisor parameter.</br>Warning: this is intrusive collection because can influence kernel page reclaim policy and add latency. Refer to https://github.com/brendangregg/wss#wsspl-referenced-page-flag for more details. | bytes | referenced_memory |
`container_sockets` | Gauge | Number of open sockets for the container | | process |
`container_spec_cpu_period` | Gauge | CPU period of the container | | - |
`container_spec_cpu_quota` | Gauge | CPU quota of the container | | - |
`container_spec_cpu_shares` | Gauge | CPU share of the container | | - |
`container_spec_memory_limit_bytes` | Gauge | Memory limit for the container | bytes | - |
`container_spec_memory_reservation_limit_bytes` | Gauge | Memory reservation limit for the container | bytes | |
`container_spec_memory_swap_limit_bytes` | Gauge | Memory swap limit for the container | bytes | |
`container_start_time_seconds` | Gauge | Start time of the container since unix epoch | seconds | |
`container_tasks_state` | Gauge | Number of tasks in given state (`sleeping`, `running`, `stopped`, `uninterruptible`, or `ioawaiting`) | | cpuLoad |
`container_threads` | Gauge | Number of threads running inside the container | | process |
`container_threads_max` | Gauge | Maximum number of threads allowed inside the container | | process |
`container_ulimits_soft` | Gauge | Soft ulimit values for the container root process. Unlimited if -1, except priority and nice | | process |

##### hardware metrics

The table below lists the Prometheus hardware metrics exposed by cAdvisor (in alphabetical order by metric name) and corresponding `-disable_metrics` / `-enable_metrics` option parameter:

Metric name | Type | Description | Unit (where applicable) | option parameter | addional build flag |
:-----------|:-----|:------------|:------------------------|:---------------------------|:--------------------
`machine_cpu_cache_capacity_bytes` | Gauge |  Cache size in bytes assigned to NUMA node and CPU core | bytes | cpu_topology |
`machine_cpu_cores` | Gauge | Number of logical CPU cores | | |
`machine_cpu_physical_cores` | Gauge | Number of physical CPU cores | | |
`machine_cpu_sockets` | Gauge | Number of CPU sockets | | |
`machine_dimm_capacity_bytes` | Gauge | Total RAM DIMM capacity (all types memory modules) value labeled by dimm type,<br>information is retrieved from sysfs edac per-DIMM API (/sys/devices/system/edac/mc/) introduced in kernel 3.6 | bytes | | |
`machine_dimm_count` | Gauge | Number of RAM DIMM (all types memory modules) value labeled by dimm type,<br>information is retrieved from sysfs edac per-DIMM API (/sys/devices/system/edac/mc/) introduced in kernel 3.6 | | |
`machine_memory_bytes` | Gauge | Amount of memory installed on the machine | bytes | |
`machine_swap_bytes` | Gauge | Amount of swap memory available on the machine | bytes | |
`machine_node_distance` | Gauge | Distance between NUMA node and target NUMA node | | cpu_topology |
`machine_node_hugepages_count` | Gauge |  Numer of hugepages assigned to NUMA node | | cpu_topology |
`machine_node_memory_capacity_bytes` | Gauge |  Amount of memory assigned to NUMA node | bytes | cpu_topology |
`machine_nvm_avg_power_budget_watts` | Gauge |  NVM power budget | watts | | libipmctl
`machine_nvm_capacity` | Gauge | NVM capacity value labeled by NVM mode (memory mode or app direct mode) | bytes | | libipmctl
`machine_thread_siblings_count` | Gauge | Number of CPU thread siblings | | cpu_topology |

#### 常用规则
#### PodCPULimitRate
pod cpu使用占limit的百分比
```
sum(rate(container_cpu_usage_seconds_total{job="cadvisor", image!="", container!="POD"}[1m])) by (cluster, namespace, pod, container) / sum(kube_pod_container_resource_limits_cpu_cores>0) by (cluster, namespace, pod, container) > 0.6
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}/container {{ $labels.container }}的CPU使用率(占limit)达{{ $value | humanizePercentage }}.

#### PodCPURequestRate
pod cpu使用占request的百分比
```
sum(rate(container_cpu_usage_seconds_total{job="cadvisor", image!="", container!="POD"}[1m])) by (cluster, namespace, pod, container) / sum(kube_pod_container_resource_requests_cpu_cores>0) by (cluster, namespace, pod, container) > 0.8
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}/container {{ $labels.container }}的CPU使用率(占request)达{{ $value | humanizePercentage }}.

#### PodMemoryLimitRate
pod 内存使用占limit的百分比
```
sum(rate(container_memory_working_set_bytes{job="cadvisor", image!="", container!="POD"}[1m])) by (cluster, namespace, pod, container) / sum(kube_pod_container_resource_limits_memory_bytes>0) by (cluster, namespace, pod, container) > 0.8
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}/container {{ $labels.container }}的内存使用率(占limit)达{{ $value | humanizePercentage }}.

#### PodMemoryRequestRate
pod 内存使用占request的百分比
```
sum(rate(container_memory_working_set_bytes{job="cadvisor", image!="", container!="POD"}[1m])) by (cluster, namespace, pod, container) / sum(kube_pod_container_resource_requests_memory_bytes>0) by (cluster, namespace, pod, container) > 0.8
```
告警内容
> 集群{{ $labels.cluster }}/namespace {{ $labels.namespace }}/Pod {{ $labels.pod }}/container{{ $labels.container }}的内存使用率(占request)达{{ $value | humanizePercentage }}.

# 参考
kubenetes runbook: https://runbooks.prometheus-operator.dev/