from folder with files run:

docker build -t jmeter-master .
docker build -t jmeter-slave .


tag all images for local registry:


docker tag jmeter-master:latest localhost:5000/jmeter-master
docker tag jmeter-slave:latest localhost:5000/jmeter-slave

push all images to local registry:

docker tag jenkins/jenkins:latest localhost:5000/jenkins
docker tag influxdb:2 localhost:5000/influxdb
docker tag portainer/agent:latest localhost:5000/portainer-agent
docker tag portainer/portainer:latest localhost:5000/portainer
docker tag grafana/grafana:9.1.6 localhost:5000/grafana
docker tag gitlab/gitlab-ce:16.4.1-ce.0 localhost:5000/gitlab

docker push localhost:5000/jenkins
docker push localhost:5000/influxdb
docker push localhost:5000/portainer-agent
docker push localhost:5000/portainer
docker push localhost:5000/grafana
docker push localhost:5000/gitlab

git push localhost:5000/jmeter-master
git push localhost:5000/jmeter-slave

check local images
http://localhost:5000/v2/_catalog

deploy stack:

docker stack deploy -c performance-stack.yml performance


new root reset gitlab in container run - gitlab-rake "gitlab:password:reset[root]"


install java for jenkins agent (your ubuntu vm)
apt-get install openjdk-11-jdk
sudo dpkg -i jdk-19.0.1_linux-x64_bin.deb
 
https://www.oracle.com/java/technologies/javase/jdk19-archive-downloads.html
jenkins agent install

jenkins master:

sudo su - jenkins
ssh-keygen -t rsa -b 4096 -m PEM
cd .ssh/
cat id_rsa.pub

copy private key to jenkins creds
sudo su - jenkins 
cat .ssh/id_rsa

jenkins slave:
sudo useradd jenkins
sudo su - jenkins

In the slave VM, add the key copied from the .ssh/rsa_pub of master to .ssh/authorized_keys of jenkins-slace

run jmeter container standalone for debugging
 docker run -dit --name master-test master-test
