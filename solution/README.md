#### Installing Docker on AWS server:

```shell
ls
apt-get update
apt-get upgrade
docker --version
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
docker --version
```

#### Cloning the git repo and pulling docker images
```shell
git clone --single-branch --branch update-assignment https://github.com/infracloudio/csvserver.git
ls
docker info
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
docker images
```

#### Creating container
```shell
docker run -id  infracloudio/csvserver /bin/bash
```

Received the below error
```logcatfilter
2023/03/24 14:11:30 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory
```




#### Created the missing file to bring the container up
```shell
docker run -p 9393:9300 -d infracloudio/csvserver:latest /bin/bash -c "touch /csvserver/inputdata; /csvserver/csvserver;"
```


#### Created gencsv.sh and running with arguments to generate "inputFile" to be used in later steps

```shell
vi gencsv.sh
chmod 777 gencsv.sh
./gencsv.sh 2 8
cat inputFile
```


#### Running the container with "inputFile" sharing the volume and passing the environment variable
```shell
docker run  -id  -p 9393:9300 -v /root/csvserver/solution:/csvserver/abc -e "CSVSERVER_BORDER=Orange" infracloudio/csvserver:latest /bin/bash -c "cp /csvserver/abc/inputFile /csvserver;mv /csvserver/inputFile /csvserver/inputdata; /csvserver/csvserver;"
```