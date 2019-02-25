Mesos + Marathon
==================

### Environment Configuration
* OS: Ubuntu 16.04 64bit
* java : 1.8
* mesos latest
* docker latest

### Install Mesos

#### install mesos master / marathon

<pre>
$ echo "{master01-private-ip} mesos-master-01" | sudo tee -a /etc/hosts
$ echo "{master02-private-ip} mesos-master-02" | sudo tee -a /etc/hosts
...

$ sudo apt-get -y install openjdk-8*
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
  DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
  CODENAME=$(lsb_release -cs)
$ echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | sudo tee /etc/apt/sources.list.d/mesosphere.list

$ sudo apt-get -y update
$ sudo apt-get install -y mesos marathon
$ echo ${ID} | sudo tee /etc/zookeeper/conf/myid
"install_mesos.md" 81L, 2733C

$ service zookeeper start
$ service mesos-master start
$ service marathon start

$ systemctl disable mesos-slave
</pre>

#### install mesos slave / docker

<pre>
$ echo "{master01-private-ip} mesos-master-01" | sudo tee -a /etc/hosts
$ echo "{master02-private-ip} mesos-master-02" | sudo tee -a /etc/hosts
...

$ sudo apt-get -y install openjdk-8*
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
  DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
  CODENAME=$(lsb_release -cs)
$ echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | sudo tee /etc/apt/sources.list.d/mesosphere.list

$ sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
  
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   xenial \
   stable"

$ sudo apt-get -y update
$ sudo apt-get install -y mesos docker-ce
$ sudo apt-get purge -y zookeeper

$ echo 'zk://mesos-master-01:2181,mesos-master-02:2181/mesos' | sudo tee /etc/mesos/zk
$ echo 'docker,mesos' | sudo tee /etc/mesos-slave/containerizers
$ echo '10mins' | sudo tee /etc/mesos-slave/executor_registration_timeout
$ echo ${slave-private-ip} | sudo tee /etc/mesos-slave/ip
$ echo ${slave-public-ip} | sudo tee /etc/mesos-slave/hostname

$ service docker start
$ service mesos-slave start

$ system enable docker
$ system enable mesos-slave
$ systemctl disable mesos-master
</pre>
