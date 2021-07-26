
[![Actions Status](https://github.com/redhat-performance/benchmark-runner/workflows/CI/badge.svg)](https://github.com/redhat-performance/benchmark-runner/actions)
[![Coverage Status](https://coveralls.io/repos/github/redhat-performance/benchmark-runner/badge.svg?branch=main)](https://coveralls.io/github/redhat-performance/benchmark-runner?branch=main)

# Benchmark-Runner

This tool provides a lightweight and flexible framework for running benchmark workloads 
on Kubernetes/OpenShift Pod or VM.

This tool support the following workloads:

* [hammerdb](https://hammerdb.com/): running hammerdb workload on the following databases: MSSQL, Mariadb, Postgresql on Pod and VM with [Configuration](benchmark_runner/benchmark_operator/templates/hammerdb)
* [stressng](https://wiki.ubuntu.com/Kernel/Reference/stress-ng): running stressng workload on Pod or VM with [Configuration](benchmark_runner/benchmark_operator/templates/stressng)
* [uperf](http://uperf.org/): running uperf workload on Pod or VM with [Configuration](benchmark_runner/benchmark_operator/templates/uperf)

** First Phase: supports [benchmark-operator workloads](https://github.com/cloud-bulldozer/benchmark-operator)

Hammerdb Kibana dashboard:
![](media/kibana.png)

Reference:
* The benchmark-runner package is located in [PyPi](https://pypi.org/project/benchmark-runner)
* The benchmark-runner container image is located in [Quay.io](https://quay.io/repository/ebattat/benchmark-runner)

![](media/docker1.png)

_**Table of Contents**_

<!-- TOC -->
- [Installation](#installation)
- [Run workload using Podman/Docker](#run-policy-using-docker-podman)
- [Run workload in Pod using Kubernetes/OpenShift](#run-policy-using-pod)
- [Post Installation](#post-installation)

<!-- /TOC -->

## Installation
**optional:**
#### Download benchmark-runner image from quay.io
```sh
sudo podman pull quay.io/ebattat/benchmark-runner:latest
```

## Run workload using Podman/Docker 

#### Environment variables description:

**mandatory:** KUBEADMIN_PASSWORD=$kubeadmin_password

**mandatory:** WORKLOAD=$workload

Choose one from the following list:

`['stressng_pod', 'stressng_vm','uperf_pod', 'uperf_vm', 'hammerdb_pod_mariadb', 'hammerdb_pod_mssql', 'hammerdb_pod_postgres', 'hammerdb_vm_mariadb', 'hammerdb_vm_mssql', 'hammerdb_vm_postgres']`

**mandatory:** PIN_NODE_BENCHMARK_OPERATOR=$pin_node_benchmark_operator [node selector for benchmark operator pod]

**mandatory:** PIN_NODE1=$pin_node1 [node1 selector for running the workload]

**mandatory:** PIN_NODE2=$pin_node2 [node2 selector for running the workload, i.e. uperf there are server and client]

**optional:** ELASTICSEARCH=$elasticsearch [ elasticsearch service name]

**optional:** ELASTICSEARCH_PORT=$elasticsearch_port

```sh
sudo podman run --rm -e WORKLOAD=stressng_pod -e KUBEADMIN_PASSWORD=$kubeadmin_password -e ELASTICSEARCH=$elasticsearch -e ELASTICSEARCH_PORT=$elasticsearch_port -e PIN_NODE1=$pin_node1 -v /root/.kube/config:/root/.kube/config -e log_level=INFO --privileged quay.io/ebattat/benchmark-runner:latest

# For custom workload data configuration add:
-v /home/user/stressng/stressng_data.yaml:/benchmark_runner/benchmark_operator/template/stressng/stressng_data.yaml
```
![](media/demo.gif)

## Run workload in Pod using Kubernetes/OpenShift

[TBD]

## Post Installation
**optional:**
#### Delete benchmark-runner image
```sh
sudo podman rmi quay.io/ebattat/benchmark-runner:latest
```
