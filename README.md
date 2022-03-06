# Setting-up-Elastic-ELK-Stack-to-AWS-EC2
## ---Steps---
## Create an EC2 instance
## Install Java
## Install Elasticsearch
## Install Logstash
## Install Kibana

### 1- Login to AWS and launch an instance
![1.png](attachment:32ee3602-293b-47a1-b75b-ec80007a47b2.png)

### 2- Select Ubuntu Server 18.04 LTS
- Ubuntu 20.04 or 18.04 LTS (64Bit x86)
![2.png](attachment:2191e541-6bbc-4fcc-8315-5fd76f487786.png)

### 3- Choose an Instance Type (Choose recommended)
- You are free to choose more than that, but less can cause problems with ELK downloads.
![3.png](attachment:61e0377c-3ad9-45b2-a89e-cef900a81201.png)

### 4- The instance Auto-assing Public IP has to be enable
![4.png](attachment:45920560-5957-452e-a5ce-23b27bbc14a6.png)

### 5- For larger size installations we can add additional volume
![5.png](attachment:fb4add6d-ad19-430d-b20b-43a80289abf1.png)

### 6- Make sure the security group type has all traffic allowed to inbound and outbound that you need
![6.png](attachment:a00a0a26-1996-48c7-9c5f-2e837a72f33d.png)

### 7- Create a Private Key file (.pem)
![Ekran Resmi 2022-02-25 01.07.46.png](attachment:1e2790b9-3d15-48a1-b710-3cf6a9d5b7b5.png)

### 8- Launch the Instance
![8.png](attachment:14d00b9b-75d6-4970-a0af-3c52df9696fd.png)

### 9- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
![Ekran Resmi 2022-02-25 01.32.26.png](attachment:3580fb2c-5545-4d99-9e52-0ab546e0d187.png)

### 10- Connect using SSH
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
![Ekran Resmi 2022-02-25 01.32.26.png](attachment:3580fb2c-5545-4d99-9e52-0ab546e0d187.png)


## Install Java
```console
sudo apt-get install default-jre
```
### Java Installation Check
```console
java -version
```

## Install Elasticsearch
### Add the repository key
```console
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
### Add default image
```console
sudo apt-get install apt-transport-https
```
### Complete the installation
```console
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```
```console
sudo apt-get update && sudo apt-get install elasticsearch
```
### Check the config file
```console
sudo nano /etc/elasticsearch/elasticsearch.yml
```
##### Remove the "#" at the beginning of the line and the parts which are in red highlights are the changes to be done.
```console
node.name: node-1
network.host: "localhost" (Your Public IPv4 DNS)
http.port: 9200
cluster.initial_master_nodes: ["node-1"]
```
### When running ElasticSearch you need to configure the JVM heap size to be on the safe side.
```console
sudo nano /etc/elasticsearch/jvm.options
```
##### Remove the "#" at the beginning of the line and the parts which are in red highlights are the changes to be done.
```console
-Xms128m
-Xmx128m
```
### Start the Elasticseach service.
```console
sudo systemctl start elasticsearch or sudo service elasticsearch start 
sudo systemctl status elasticsearch or sudo service elasticsearch status
```
### CURL command.
```console
curl http://<Your Public IPv4 DNS>:9200 
```

### To stop elasticsearch after installing all stacks:
```console
sudo systemctl stop elastic search
sudo service stop elastic call
```




## Install Logstash
```console
sudo apt-get install logstash
```
### Create a config file.
```console
sudo nano /etc/logstash/conf.d/apache-01.conf
```
##### Write into:
```console
input {
        file {
                path => "/var/log/messages", "/var/log/*.log"
                start_position => "beginning"
        }
}
output{
        elasticsearch {
                hosts => ["<Your Public IPv4 DNS>:9200"]
        }
}
```

### Start the Logstash service.
```console
sudo systemctl start logstash or sudo service logstash start
```

### CURL command.
#### If you get an error in this part, install the "sudo apt install libwww-perl" file.
```console
sudo curl -XGET '<Your Public IPv4 DNS>:9200/_cat/indices?v&pretty'
```


## Install Kibana
```console
sudo apt-get install kibana
```

### Check the config file
```console
sudo nano /etc/kibana/kibana.yml
```
##### Remove the "#" at the beginning of the line and the parts which are in red highlights are the changes to be done.
```console
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://<Your Public IPv4 DNS>:9200"]
```
### Start the Kibana service.
```console
sudo service kibana start
```

### Follow the url with your public IP and the port 5601.
```console
http://<Your Public IPv4 DNS>:5601
```


    
    
## Seda Atalay
