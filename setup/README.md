## Learn the Elastic Stack at Home
Run the Elastic Stack on Docker or run the Elastic Stack on virtual machine(s).

For VMs, use your prefered hypervisor.  The instructions below are for CentOS 7 on VMware ESXi.

To keep it simple, we will run a single node cluster.

### Using the Elastic Stack on Docker
#### Download [Docker Desktop](https://docs.docker.com/desktop/):
* Docker Desktop for Windows:  https://docs.docker.com/docker-for-windows/install/
  * Users running Docker Desktop will need to be part of the `docker-users` group
  * Once Docker is installed, open PowerShell (not PowerShell ISE)
  * In PowerShell, issue `docker --version` to ensure Docker is running

* Pre-download the Elasticsearch and Kibana Docker containers:
  * In PowerShell, issue the following commands:
    * `docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2`
    * `docker pull docker.elastic.co/kibana/kibana:7.6.2`



### Using the Elastic Stack on VMs
_this section is in progress_
