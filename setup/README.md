## Learn the Elastic Stack at Home
Run the Elastic Stack on Docker.

**_DISCLAIMER_**: These instructions are meant to be used for learning/training ONLY and do not include configuring TLS/SSL with the Elastic Stack.

To keep it simple, we will run a single node cluster.

If you would prefer to run the Elastic Stack on VMs, see: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html

### Using the Elastic Stack on Docker
Reference: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html
#### Download [Docker Desktop](https://docs.docker.com/desktop/):
* Docker Desktop for Windows:  https://docs.docker.com/docker-for-windows/install/
  * Users running Docker Desktop will need to be part of the `docker-users` group
  * Once Docker is installed, open PowerShell (not PowerShell ISE)
  * In PowerShell, issue `docker --version` to ensure Docker is running

#### Get Ready
* Pre-download the Elasticsearch and Kibana Docker containers:
  * In PowerShell, issue the following commands:
    * `docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2`
    * `docker pull docker.elastic.co/kibana/kibana:7.6.2`
  * Download the `docker-compose.yml` file from this directory
    * Store it in a location you will remember
    * For example, store the file `C:\temp\learnElasticStack`
* Steps:
  * `PS C:\Users\myUser> docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2`
  * `PS C:\Users\myUser> docker pull docker.elastic.co/kibana/kibana:7.6.2`
  * `PS C:\Users\myUser> cd C:\temp\`
  * `PS C:\temp> mkdir learnElasticStack`
  * `PS C:\temp> cp C:\Users\myUser\Downloads\docker-compose.yml .\learnElasticStack\docker-compose.yml`

#### Run the Elastic Stack on Docker
* In PowerShell, navigate to `C:\temp\learnElasticStack`
  * Issue the command: `docker-compose up`
  * Look for the following message from `kib01`: `"message":"http server running at http://0:5601"`
  * Open your web browser and navigate to `http://localhost:5601/`
* Steps:
  * `PS C:\Users\myUser> cd C:\temp\learnElasticStack\`
  * `PS C:\temp\learnElasticStack> docker-compose up`

### Send Beats data to your cluster
Reference: https://ela.st/siem-for-home

#### Configure the GeoIP ingest pipeline
Reference: https://www.elastic.co/blog/elastic-siem-for-small-business-and-home-3-geoip-data-and-beats-config-review

#### Install Beats on your local device
We will follow the specific _Beats on Windows, CentOS, MacOS_ blog, with a few modifications to the _Beats Common Configurations_ file.
* Beats Config: https://www.elastic.co/blog/elastic-siem-for-small-business-and-home-3-geoip-data-and-beats-config-review
* Windows: https://www.elastic.co/blog/elastic-siem-for-small-business-and-home-4-beats-on-windows
* CentOS: https://www.elastic.co/blog/elastic-siem-for-small-business-and-home-5-beats-on-centos
* macOS: https://www.elastic.co/blog/elastic-siem-for-small-business-and-home-6-beats-on-mac

Instead of configuring the `Elastic Cloud` output, we will use the `Elasticsearch` output.

From https://github.com/elastic/examples/blob/master/Security%20Analytics/SIEM-at-Home/beats-configs/beats-general-config.yml , we will modify the following section:
```
#=== Elastic Cloud ===
# These settings simplify using beats with the Elastic Cloud (https://cloud.elastic.co/).
cloud.id: "My_Elastic_Cloud_Deployment:abcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ"
cloud.auth: "data_shipper:0987654321abcDEF"
```

Change to:
```
#=== Elasticsearch ===
output.elasticsearch:
  hosts: ["localhost"]
```
