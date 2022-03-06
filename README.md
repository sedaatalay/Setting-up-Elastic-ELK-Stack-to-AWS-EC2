# Setting-up-Elastic-(ELK)-Stack-to-AWS-EC2
## ---Steps---
## Create an EC2 instance
## Install Java
## Install Elasticsearch
## Install Logstash
## Install Kibana

<p>  <br />
</p>

## Create an EC2 instance
### Login to AWS and launch an instance
<img width="1234" alt="Ekran Resmi 2022-02-27 20 40 21" src="https://user-images.githubusercontent.com/91700155/156904529-90090427-d4a5-40c5-9349-c766d37a71ad.png">

### Select Ubuntu Server 18.04 LTS
- Ubuntu 20.04 or 18.04 LTS (64Bit x86)
<img width="1407" alt="Ekran Resmi 2022-02-27 20 41 24" src="https://user-images.githubusercontent.com/91700155/156904538-05a49df6-75d7-43c3-a0f6-dc6ca0750305.png">

### Choose an Instance Type (Choose recommended)
- You are free to choose more than that, but less can cause problems with ELK downloads.
<img width="1429" alt="Ekran Resmi 2022-02-27 20 41 58" src="https://user-images.githubusercontent.com/91700155/156904545-ca9ff679-2756-44b6-8efe-559e8e248573.png">

### The instance Auto-assing Public IP has to be enable
<img width="1431" alt="Ekran Resmi 2022-02-27 20 42 23" src="https://user-images.githubusercontent.com/91700155/156904549-d8b3459f-2ff5-40dc-aadb-e50ac811e020.png">

### For larger size installations we can add additional volume
<img width="1428" alt="Ekran Resmi 2022-02-27 20 42 58" src="https://user-images.githubusercontent.com/91700155/156904562-11a127af-98bd-47fb-b1b5-1dbe096d6ac5.png">

### Make sure the security group type has all traffic allowed to inbound and outbound that you need
<img width="1429" alt="Ekran Resmi 2022-03-06 01 45 50" src="https://user-images.githubusercontent.com/91700155/156904280-694584de-b0e6-46f4-83e2-76cda9330164.png">

### Create a Private Key file (.pem)
<img width="1413" alt="Ekran Resmi 2022-03-06 01 46 54" src="https://user-images.githubusercontent.com/91700155/156904286-e1e9f6c8-37ad-4d60-9631-5b5cca393499.png">

### Launch the Instance
<img width="1413" alt="Ekran Resmi 2022-03-06 01 47 18" src="https://user-images.githubusercontent.com/91700155/156904289-7f53b110-47a5-42c7-860e-9b938ed880bf.png">
<img width="918" alt="Ekran Resmi 2022-03-06 01 48 14" src="https://user-images.githubusercontent.com/91700155/156904296-c54ef3b4-0cd6-40de-9f58-20a1263be57b.png">

### Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
<img width="569" alt="Ekran Resmi 2022-03-06 01 52 09" src="https://user-images.githubusercontent.com/91700155/156904333-39bb747a-30b1-4a29-9b3e-1fb1dc9be061.png">


### Connect using SSH
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
<img width="814" alt="Ekran Resmi 2022-03-06 01 52 09" src="https://user-images.githubusercontent.com/91700155/156904342-ddb0507e-ba74-40ae-baa1-a6f9f007a9c8.png">

<p>  <br />
</p>

## Install Java
```console
sudo apt-get install default-jre
```
### Java Installation Check
```console
java -version
```
<img width="1029" alt="Ekran Resmi 2022-03-06 01 53 40" src="https://user-images.githubusercontent.com/91700155/156904352-cc9a2243-7a97-446a-9e94-11c77fe9b5eb.png">

<p>   <br />
</p>
        
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
<img width="947" alt="Ekran Resmi 2022-03-06 01 56 00" src="https://user-images.githubusercontent.com/91700155/156904368-49bcc451-4cce-4b6a-9c01-7defc8f09514.png">

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
<img width="879" alt="Ekran Resmi 2022-03-06 02 03 15" src="https://user-images.githubusercontent.com/91700155/156904390-24477f50-6306-4eb0-8d37-d2d4812721d1.png">

<img width="879" alt="Ekran Resmi 2022-03-06 02 06 37" src="https://user-images.githubusercontent.com/91700155/156904391-55ecf900-a941-435b-93db-c5e551834570.png">

### When running ElasticSearch you need to configure the JVM heap size to be on the safe side.
```console
sudo nano /etc/elasticsearch/jvm.options
```

##### Remove the "#" at the beginning of the line and the parts which are in red highlights are the changes to be done.
```console
-Xms128m
-Xmx128m
```
<img width="874" alt="Ekran Resmi 2022-03-06 02 11 41" src="https://user-images.githubusercontent.com/91700155/156904396-1b80f087-f497-4580-af5c-c62294d42ce4.png">


### Start the Elasticseach service.
```console
sudo systemctl start elasticsearch or sudo service elasticsearch start 
sudo systemctl status elasticsearch or sudo service elasticsearch status
```
<img width="879" alt="Ekran Resmi 2022-03-06 02 13 34" src="https://user-images.githubusercontent.com/91700155/156904424-5a37c3b4-5634-4cb1-911f-fdc88aac2bd4.png">

### CURL command.
```console
curl http://<Your Public IPv4 DNS>:9200 
```
<img width="865" alt="Ekran Resmi 2022-03-06 02 14 50" src="https://user-images.githubusercontent.com/91700155/156904428-5329bbdd-53de-4613-956d-089f473effda.png">

<img width="1405" alt="Ekran Resmi 2022-03-06 02 15 16" src="https://user-images.githubusercontent.com/91700155/156904430-5c99fa0c-f1da-49a3-9e0d-d3a5ab630adb.png">

### To stop elasticsearch after installing all stacks:
```console
sudo systemctl stop elastic search
sudo service stop elastic call
```

<p>  <br />
</p>

## Install Logstash
```console
sudo apt-get install logstash
```
<img width="881" alt="Ekran Resmi 2022-03-06 02 16 44" src="https://user-images.githubusercontent.com/91700155/156904444-c89cc1d5-a2dd-45b3-93e2-df0766e315e5.png">

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
<img width="880" alt="Ekran Resmi 2022-03-06 02 18 02" src="https://user-images.githubusercontent.com/91700155/156904455-2fb32809-0378-4412-a7dc-e491f3ca439f.png">

<img width="856" alt="Ekran Resmi 2022-03-06 02 18 53" src="https://user-images.githubusercontent.com/91700155/156904462-cb14dc9b-63c1-46ff-b54a-6825b5685aa2.png">

### Start the Logstash service.
```console
sudo systemctl start logstash or sudo service logstash start
```

### CURL command.
#### If you get an error in this part, install the "sudo apt install libwww-perl" file.
```console
sudo curl -XGET '<Your Public IPv4 DNS>:9200/_cat/indices?v&pretty'
```
<img width="955" alt="Ekran Resmi 2022-03-06 02 19 44" src="https://user-images.githubusercontent.com/91700155/156904467-9dafadd1-8f78-476a-8864-4159ac66dcfc.png">

<p>  <br />
</p>

## Install Kibana
```console
sudo apt-get install kibana
```
<img width="987" alt="Ekran Resmi 2022-03-06 02 21 22" src="https://user-images.githubusercontent.com/91700155/156904477-5be9739f-f180-41e1-93d7-02f69c3bd86e.png">

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
<img width="952" alt="Ekran Resmi 2022-03-06 02 23 16" src="https://user-images.githubusercontent.com/91700155/156904491-3baae4da-7b70-49bf-a6a2-ed4e137ff96b.png">

### Start the Kibana service.
```console
sudo service kibana start
```
<img width="952" alt="Ekran Resmi 2022-03-06 02 24 41" src="https://user-images.githubusercontent.com/91700155/156904501-5300ce70-91a2-4aea-a892-f83d839419cb.png">

### Follow the url with your public IP and the port 5601.
```console
http://<Your Public IPv4 DNS>:5601
```
<img width="1398" alt="Ekran Resmi 2022-03-06 02 25 27" src="https://user-images.githubusercontent.com/91700155/156904507-33203d0d-3f9d-4aca-9e35-b0c9fe861b42.png">


<p>  <br />
</p>
    
#### Seda Atalay
