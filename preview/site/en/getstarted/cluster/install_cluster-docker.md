---
id: install_cluster-docker.md
title: Install Milvus Cluster
label: Install with Docker
order: 0
group: cluster
---

# Install Milvus Cluster

## Before You Begin

Before moving forward to installation, you must check the eligibility of your hardware in line with Milvus' requirement.

<details><summary>Check your Docker and Docker Compose version</summary>

<div class="alert note">
Docker Compose is the recommended way to install Milvus.
</div>

- Docker version 19.03 or higher is required.
- Docker Compose version 1.25.1 or higher is required. 
</details>

<details><summary>Check whether your CPU supports SIMD extension instruction set</summary>

Milvus' computing operations depend on CPU’s support for SIMD (Single Instruction, Multiple Data) extension instruction set. Whether your CPU supports SIMD extension instruction set is crucial to index building and vector similarity search within Milvus. Ensure that your CPU supports at least one of the following SIMD instruction sets:

- SSE4.2
- AVX
- AVX2
- AVX512

Run the lscpu command to check if your CPU supports the SIMD instruction sets mentioned above:

```
$ lscpu | grep -e sse4_2 -e avx -e avx2 -e avx512
```
</details>

<details><summary>Check your GPU’s eligibility</summary>
Milvus Cluster supports GPU acceleration on floating vectors. 
- Supported Nvidia GPU versions are 6.0, 6.1, 7.0, and 7.5.

<div class="alert note">
You can access <a href="https://developer.nvidia.com/cuda-gpus">Nvidia</a>'s official website to check the corresponding version of your GPU. 
</div>

- Milvus requires [CUDA version 10.0 or higher](https://developer.nvidia.com/cuda-10.0-download-archive). 

<div class="alert note">
Enabling GPU acceleration in Milvus is optional. You can still run the whole Milvus service even if your server does not have a GPU device.
</div>

In the current version, the following vector indexes support GPU acceleration:

- FLAT
- IVF-FLAT
- IVF-SQ8
- IVF-PQ

Learn more about [Vector indexes](https://www.zilliz.com/blog/Accelerating-Similarity-Search-on-Really-Big-Data-with-Vector-Indexing#flat-good-for-searching-relatively-small-million-scale-datasets-when-100-recall-is-required).

</details>

## Install Milvus Cluster

<div class="tab-wrapper"><a href="install_cluster-docker.md" class='active '>Install with Docker</a><a href="install_cluster-helm.md" class=''>Install with Helm Chart</a></div>

1. Docker version 19.03 or higher is required. Check Docker version:

```
$ sudo docker info
```

> Follow [Get Docker](https://docs.docker.com/get-docker/) to install Docker on your system.

2. Docker Compose version 1.25.1 or higher is required. Check Docker Compose version:

```
$ sudo docker-compose version
```

> See [Install Docker Compose](https://docs.docker.com/compose/install/) for Docker Compose installation guide.

3. Download **docker-compose.yml**.

```
$ mkdir -p /home/$USER/milvus
$ cd home/$USER/milvus
$ wget https://raw.githubusercontent.com/milvus-io/milvus/v2.0.0/deployments/docker/distributed/docker-compose.yml
```
4. Start Docker Compose.
```
$ sudo docker-compose up -d 
```

*If Docker Compose boots successfully, 12 running docker containers will appear (nine infrastructure services and three Milvus services):*

<code>
$ sudo docker ps 

<table>
    <tr>
        <td>CONTAINER ID</td>
        <td>IMAGE</td>
        <td>COMMAND</td>
        <td>CREATED</td>
        <td>STATUS</td>
        <td>PORTS</td>
        <td>NAMES</td>
    </tr>
    <tr>
        <td>0f9d37d78e0c</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_querynode_1</td>
    </tr>
    <tr>
        <td>40568c5d5c40</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_indexnode_1</td>
    </tr>
    <tr>
        <td>071124ad8e1a</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_datanode_1</td>
    </tr>
    <tr>
        <td>22d4786a6b22</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td>0.0.0.0:19530-&gt;19530/tcp, :::19530-&gt;19530/tcp</td>
        <td>distributed_proxynode_1</td>
    </tr>
    <tr>
        <td>f92daa379628</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_indexservice_1</td>
    </tr>
    <tr>
        <td>5d592010b3aa</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_master_1</td>
    </tr>
    <tr>
        <td>481bae1480ea</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_queryservice_1</td>
    </tr>
    <tr>
        <td>d87fe6b9d731</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_proxyservice_1</td>
    </tr>
    <tr>
        <td>7513e26e1ee2</td>
        <td>milvusdb/milvus:2.0.0-d043021-19c36b</td>
        <td>"/tini -- /milvus/bi…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_dataservice_1</td>
    </tr>
    <tr>
        <td>75d4ff2916b7</td>
        <td>minio/minio:RELEASE.2020-12-03T00-03-10Z</td>
        <td>"/usr/bin/docker-ent…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes (healthy)</td>
        <td>9000/tcp</td>
        <td>distributed_minio_1</td>
    </tr>
    <tr>
        <td>08b81e680c82</td>
        <td>quay.io/coreos/etcd:latest</td>
        <td>"etcd -listen-peer-u…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td>2379-2380/tcp</td>
        <td>distributed_etcd_1</td>
    </tr>
    <tr>
        <td>5622c872ed3e</td>
        <td>apachepulsar/pulsar:latest</td>
        <td>"bin/pulsar standalo…"</td>
        <td>7 minutes ago</td>
        <td>Up 7 minutes</td>
        <td></td>
        <td>distributed_pulsar_1</td>
    </tr>
</table>
</code>

> To stop Docker Compose, run ```$ sudo docker-compose down```.