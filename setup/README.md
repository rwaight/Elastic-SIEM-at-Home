## Learn the Elastic Stack at Home
Run the Elastic Stack on Docker or on a Virtual Machine.

**_DISCLAIMER_**: These instructions are meant to be used for learning/training ONLY and do not include configuring TLS/SSL with the Elastic Stack.

To keep it simple, we will run a single node cluster.

If you would prefer to run the Elastic Stack on VMs, see: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html

## Run the Elastic Stack on Docker
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

#### Starting the Elastic Stack on Docker
* In PowerShell, navigate to `C:\temp\learnElasticStack`
  * Issue the command: `docker-compose up`
  * Look for the following message from `kib01`: `"message":"http server running at http://0:5601"`
  * Open your web browser and navigate to `http://localhost:5601/`
* Steps:
  * `PS C:\Users\myUser> cd C:\temp\learnElasticStack\`
  * `PS C:\temp\learnElasticStack> docker-compose up`

## Run the Elastic Stack on a Virtual Machine
Reference: https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html
#### Build/deploy a VM on the hypervisor of your choice:
* Linux VM Specs:
  * 4GB RAM (8GB recommended)
  * 2 vCPU (4 vCPU recommended)
  * 40GB HD
  * Network access to host machine

#### Get Ready
* Download CentOS: https://www.centos.org/download/
  * CentOS Linux DVD ISO: http://isoredirect.centos.org/centos/8/isos/x86_64/CentOS-8.1.1911-x86_64-dvd1.iso
* Start the VM and install CentOS
* Sign in as `root` (or an administrative user)
* Update CentOS and install Java
```
yum -y update
yum -y install java
```
* Install OpenVM Tools
```
yum -y install open-vm-tools
systemctl enable vmtoolsd.service
systemctl start vmtoolsd
```

#### Install Elasticsearch and Kibana
* References:
  * Elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/7.6/rpm.html
  * Kibana: https://www.elastic.co/guide/en/kibana/7.6/rpm.html
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
cd /etc/yum.repos.d/
elr='elasticsearch.repo'
echo "Creating file $elr and populating it with 7.x info"
echo "[elasticsearch-7.x]" >> $elr
echo "name=Elasticsearch repository for 7.x packages" >> $elr 
echo "baseurl=https://artifacts.elastic.co/packages/7.x/yum" >> $elr
echo "gpgcheck=1" >> $elr
echo "gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch" >> $elr
echo "enabled=1" >> $elr 
echo "autorefresh=1" >> $elr
echo "type=rpm-md" >> $elr
echo "Populated $elr with data"

cd /home/
  
yum -y install elasticsearch
cd /etc/elasticsearch/
cp elasticsearch.yml elasticsearch.yml.backup
cp jvm.options jvm.options.backup
echo "Elasticsearch config needs to be updated, path is /etc/elasticsearch/elasticsearch.yml"
echo "JVM config needs to be updated, path is /etc/elasticsearch/jvm.options"
#systemctl enable elasticsearch.service # Do not automatically enable Elasticsearch

cd /home/

yum -y install kibana
cd /etc/kibana/
cp kibana.yml kibana.yml.backup
echo "Kibana config needs to be updated, path is /etc/kibana/kibana.yml"
# systemctl enable kibana.service # Do not automatically enable Kibana
cd /home/
```
#### Update CentOS Firewall
```
firewall-cmd --zone=public --add-port=9200/tcp --permanent
firewall-cmd --zone=public --add-port=9300/tcp --permanent
echo "Default ports opened in the public zone for Elasticsearch"
firewall-cmd --zone=public --add-service=elasticsearch --permanent
echo "Elasticsearch service has been added to the public zone"
systemctl reload firewalld

firewall-cmd --zone=public --add-port=5601/tcp --permanent
echo "Default ports opened in the public zone for Kibana"
firewall-cmd --zone=public --add-service=kibana --permanent
echo "Kibana service has been added to the public zone"
systemctl reload firewalld
```

#### Configure and start Elasticsearch and Kibana
* Update the `elasticsearch.yml` configuration file and the `jvm.options` file
```
cd /etc/elasticsearch/
vi elasticsearch.yml
vi jvm.options
```
* Enable and start Elasticsearch
```
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```
* Update the `kibana.yml` configuration file
```
cd /etc/kibana/
vi kibana.yml
```
* Enable and start Kibana
```
systemctl enable kibana.service
systemctl start kibana.service
```


## Send Beats data to your cluster
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
# For the Stack on docker, use `localhost`
# For the Stack on a VM, use the IP of your VM
output.elasticsearch:
  #hosts: ["localhost"]
  # or
  #hosts: ["ip_of_your_VM"]
```
