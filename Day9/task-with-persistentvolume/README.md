# Day 3

## Finding the shared folders in your NFS server
```
showmount -e localhost
```

Expected output
<pre>
(jegan@tektutor.org)$ showmount -e localhost
Export list for localhost:
/mnt/pv5        192.168.122.0/24
/mnt/pv4        192.168.122.0/24
/mnt/pv3        192.168.122.0/24
/mnt/pv2        192.168.122.0/24
/mnt/pv1        192.168.122.0/24
/mnt/share2     192.168.122.0/24
/mnt/share1     192.168.122.0/24
/mnt/git-source 192.168.122.0/24
/mnt/gitea      192.168.122.0/24
/mnt/maven      192.168.122.0/24
/mnt/wordpress  192.168.122.0/24
/mnt/mysql      192.168.122.0/24
/mnt/nfs_share  192.168.122.0/24
</pre>


## ⛹️‍♀️ Lab - Deploying mysql database without using external storage

If we deploy mysql using Pod storage, when Pod is deleted the data will be lost.

```
oc delete project jegan

oc new-project jegan
oc create deployment mysql --image=bitnami/mysql:latest
oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Already on project "jegan" on server "https://api.ocp.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname

(jegan@tektutor.org)$ <b>oc create deploy mysql --image=bitnami/mysql:latest</b>
deployment.apps/mysql created
(jegan@tektutor.org)$ <b>oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root</b>
deployment.apps/mysql updated
</pre>

### Getting inside the mysql shell

When prompts for password, type 'root' 
```
oc rsh deploy/mysql 

mysql -u root -p
SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE training ( id INT, name VARCHAR(50), duration VARCHAR(50) );
INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO training VALUES ( 2 "TekTon with OpenShift", "5 Days" );
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc rsh deploy/mysql</b>
$ <b>mysql -u root -p</b>
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.30 Source distribution

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> <b>SHOW DATABASES;</b>
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> <b>CREATE DATABASE tektutor;</b>
Query OK, 1 row affected (0.01 sec)

mysql> <b>USE tektutor;</b>
Database changed
mysql> <b>CREATE TABLE training ( id INT, name VARCHAR(50), duration VARCHAR(50) );</b>
Query OK, 0 rows affected (0.04 sec)

mysql> <b>INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );</b>
Query OK, 1 row affected (0.01 sec)

mysql> <b>INSERT INTO training VALUES ( 2, "Tekton with OpenShift", "5 Days" );</b>
Query OK, 1 row affected (0.01 sec)

mysql> <b>SELECT * FROM training;</b>
+------+-----------------------+----------+
| id   | name                  | duration |
+------+-----------------------+----------+
|    1 | DevOps                | 5 Days   |
|    2 | Tekton with OpenShift | 5 Days   |
+------+-----------------------+----------+
2 rows in set (0.00 sec)

mysql> <b>exit</b>
Bye
$ <b>exit</b>
</pre>

## ⛹️‍♀️ Lab - Deploying mysql database with external storage(Persistent Volume)

### Clone the TekTutor GitHub Repository

You need to edit the YAML files in Day3, replacing 'jegan' with your name before you proceed.

In the mysql-pv.yml file, you need to edit the mount path to /mnt/<your-linux-user> and update the IP address with your openshift cluster ip.

```
cd ~
git clone https://github.com/tektutor/openshift-sep-2022-batch2.git
cd openshift-sep-2022-batch2
git pull

cd Day3
oc delete deploy/mysql

oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deploy.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ oc apply -f mysql-pv.yml 
persistentvolume/mysql-pv-jegan created

(jegan@tektutor.org)$ oc apply -f mysql-pvc.yml
persistentvolumeclaim/mysql-pvc-jegan created

(jegan@tektutor.org)$ oc apply -f mysql-deploy.yml 
deployment.apps/mysql created
</pre>

Get inside the mysql pod shell ( You need to type root as password when promtped )
```
oc rsh deploy/mysql
mysql -u root -p
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE training ( id INT, name VARCHAR(50), duration VARCHAR(50) );
INSERT INTO training VALUES ( 1, "TekTon with OpenShift", "5 Day" );
INSERT INTO training VALUES ( 2, "OpenShift", "5 Day" );
SELECT * FROM training;
```

Expected output
<pre>

</pre>

## ⛹️ Lab - Deploying application from a Dockerfile in GitHub
```
oc delete project jegan
oc new-project jegan

oc new-app --name=hello https://github.com/tektutor/spring-ms.git
oc expose deploy/hello
oc expose svc/hello
```

## ⛹️ Lab - Deploying application from source code cloned from GitHub (Strategy S2I - Source to Image)
```
oc delete project jegan
oc new-project jegan

oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-springboot-ms.git

oc logs -f bc/hello

oc expose deploy/hello
oc expose svc/hello
```

Expected output
<pre>
(jegan@tektutor.org)$ oc new-app --name hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git
--> Found container image 2e897fa (8 days old) from registry.redhat.io for "registry.redhat.io/ubi8/openjdk-11:latest"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * An image stream tag will be created as "openjdk-11:latest" that will track the source image
    * A source build using source code from https://github.com/tektutor/hello-spring-boot.git will be created
      * The resulting image will be pushed to image stream tag "hello:latest"
      * Every time "openjdk-11:latest" changes a new build will be triggered

--> Creating resources ...
    imagestream.image.openshift.io "openjdk-11" created
    imagestream.image.openshift.io "hello" created
    buildconfig.build.openshift.io "hello" created
    deployment.apps "hello" created
    service "hello" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/hello' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/hello' 
    Run 'oc status' to view your app.
</pre>
