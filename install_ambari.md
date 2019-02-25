Ambari
===============

### Environment Configuration
* OS: Red Hat Enterprise Linux (RHEL) v7.x (64-bit) [aws ec2]
* package: wget, curl, scp, unzip, tar, pdsh
* java: 1.8


### Install Ambari

#### default setting..
<pre>
$ sudo yum install -y wget curl scp unzip tar pdsh
$ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "https://download.oracle.com/otn-pub/java/jdk/8u201-b09/42970487e3af4f5aa5bca3f542482c60/jdk-8u201-linux-x64.rpm"
$ sudo rpm -ivh jdk-8u201-linux-x64.rpm
$ sudo yum-config-manager --enable rhui-REGION-rhel-server-optional
$ sudo yum install libtirpc-devel

$ sudo vi /etc/hosts
{cluster-private-ip} ambari-cluster-01
{cluster-private-ip} ambari-cluster-02
....
</pre>

#### setting password-less ssh
<pre>
$ sudo chmod 600 ~/.ssh/{permission-file}.pem
$ ssh-keygen -t rsa -N "" -f /home/ec2-user/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/{permission-file}.pem ec2-user@{ambari-cluster-private-ip} "cat >> ~/.ssh/authorized_keys"
</pre>

##### check ssl
<pre>
$ ssh ambari-cluster-01
$ ssh ambari-cluster-02
...
</pre>

#### install ambari server/agent
<pre>
$ sudo wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.1.0/ambari.repo -O /etc/yum.repos.d/ambari.repo

// install ambari server
$ sudo yum install ambari-server 
$ sudo ambari-server setup 
$ sudo ambari-server start 

// install ambari agent
$ sudo yum install ambari-agent 
$ sudo ambari-agent start
</pre>


