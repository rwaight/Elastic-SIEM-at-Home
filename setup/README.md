## Learn the Elastic Stack at Home
Run the Elastic Stack on Docker or run the Elastic Stack on virtual machine(s).

**_DISCLAIMER_**: These instructions are meant to be used for learning/training ONLY and do not include configuring TLS/SSL with the Elastic Stack.

For VMs, use your prefered hypervisor.  The instructions below are for CentOS 7 on VMware ESXi.

To keep it simple, we will run a single node cluster.

### Using the Elastic Stack on Docker
Reference: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html
#### Download [Docker Desktop](https://docs.docker.com/desktop/):
* Docker Desktop for Windows:  https://docs.docker.com/docker-for-windows/install/
  * Users running Docker Desktop will need to be part of the `docker-users` group
  * Once Docker is installed, open PowerShell (not PowerShell ISE)
  * In PowerShell, issue `docker --version` to ensure Docker is running

* Pre-download the Elasticsearch and Kibana Docker containers:
  * In PowerShell, issue the following commands:
    * `docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2`
    * `docker pull docker.elastic.co/kibana/kibana:7.6.2`
  * Download the `docker-compose.yml` file from this directory
    * Store it in a location you will remember
    * For example, store the file `C:\temp\learnElasticStack`


### Using the Elastic Stack on VMs
Reference: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html

#### _this section is in progress_
