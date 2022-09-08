# Day 4

## ⛹️‍♂️ Lab - Deploying application from existing Container Image
```
oc delete project jegan

oc new-project jegan
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080
oc expose svc/nginx
```

## ⛹️‍♀️ Lab - Deploying application from Dockerfile from a GitHub Repo
```
oc delete deploy/hello

oc new-app --name=hello https://github.com/tektutor/spring-ms.git
```


## ⛹️ Lab - Deploying application from source code from a GitHub repo
```
oc delete deploy/hello

oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc new-app --name=hello registry.redhat.io/ubi8/openjdk-11:latest~https://github.com/tektutor/hello-spring-boot.git</b>
--> Found container image 2e897fa (9 days old) from registry.redhat.io for "registry.redhat.io/ubi8/openjdk-11:latest"

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
(jegan@tektutor.org)$ <b>oc logs -f buildconfig/hello</b>
</pre>

## ⛹️‍♀️ Lab - Deploying mysql database server into OpenShift cluster
```
oc create deployment mysql --image=bitnami/mysql:latest
oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create deployment mysql --image=bitnami/mysql:latest</b>
deployment.apps/mysql created
(jegan@tektutor.org)$ <b>oc get deploy,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql   0/1     1            0           12s

NAME                         READY   STATUS              RESTARTS   AGE
pod/mysql-68bc9bf65d-68c76   0/1     ContainerCreating   0          12s
(jegan@tektutor.org)$ <b>oc get po -w</b>
NAME                     READY   STATUS              RESTARTS   AGE
mysql-68bc9bf65d-68c76   0/1     ContainerCreating   0          20s
mysql-68bc9bf65d-68c76   0/1     Error               0          27s
^C(jegan@tektutor.org)$ <b>oc logs mysql-68bc9bf65d-68c76</b>
mysql 05:10:06.25 
mysql 05:10:06.25 Welcome to the Bitnami mysql container
mysql 05:10:06.26 Subscribe to project updates by watching https://github.com/bitnami/containers
mysql 05:10:06.26 Submit issues and feature requests at https://github.com/bitnami/containers/issues
mysql 05:10:06.26 
mysql 05:10:06.27 INFO  ==> ** Starting MySQL setup **
mysql 05:10:06.30 INFO  ==> Validating settings in MYSQL_*/MARIADB_* env vars
mysql 05:10:06.31 ERROR ==> The MYSQL_ROOT_PASSWORD environment variable is empty or not set. Set the environment variable ALLOW_EMPTY_PASSWORD=yes to allow the container to be started with blank passwords. This is recommended only for development.
(jegan@tektutor.org)$ <b>oc set env deploy/mysql MYSQL_ROOT_PASSWORD=root</b>
deployment.apps/mysql updated
(jegan@tektutor.org)$ <b>oc get po -w</b>
NAME                     READY   STATUS              RESTARTS      AGE
mysql-68bc9bf65d-68c76   0/1     Error               3 (37s ago)   89s
mysql-98798fb95-bgcph    0/1     ContainerCreating   0             3s
<b>mysql-98798fb95-bgcph    1/1     Running             0             6s</b>
mysql-68bc9bf65d-68c76   0/1     Terminating         3 (40s ago)   92s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             92s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             93s
mysql-68bc9bf65d-68c76   0/1     Terminating         3             93s
</pre>


## Lab - Creating database, table and inserting records into Pod storage

When prompts for password, you need to type the password you passed via the environment variable MYSQL_ROOT_PASSWORD. In my case, my password is 'root' without quotes.

```
oc rsh deploy/mysql

mysql -u root -p

SHOW DATABASES;

CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE training ( id INT, name VARCHAR(50), duration VARCHAR(50) );

INSERT INTO training VALUES ( 1, "Machine Learning with Python", "5 Days" );
INSERT INTO training VALUES ( 2, "Game Programing with QML" , "5 Days" );

SELECT * FROM training;
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc rsh deploy/mysql</b>
$ <b>mysql -u root -p</b>
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
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
4 rows in set (0.00 sec)

mysql> <b>CREATE DATABASE tektutor;</b>
Query OK, 1 row affected (0.00 sec)

mysql> <b>USE tektutor;</b>
Database changed
mysql> <b>CREATE TABLE training ( id INT, name VARCHAR(50), duration VARCHAR(50) );</b>
Query OK, 0 rows affected (0.04 sec)

mysql> <b>INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );</b>
Query OK, 1 row affected (0.02 sec)

mysql> <b>INSERT INTO training VALUES ( 2, "Datastructures and Algorithms", "5 Days" );</b>
Query OK, 1 row affected (0.00 sec)

mysql> <b>SELECT * FROM training;</b>
+------+-------------------------------+----------+
| id   | name                          | duration |
+------+-------------------------------+----------+
|    1 | DevOps                        | 5 Days   |
|    2 | Datastructures and Algorithms | 5 Days   |
+------+-------------------------------+----------+
2 rows in set (0.00 sec)

mysql> <b>exit</b>
Bye
$ <b>exit</b>
</pre>



## Deploying mysql db server in declarative style using manifest(yaml) file

### Delete any existing mysql deployment
```
oc delete deploy/mysql
```

Let's deploy mysql db server declaratively now
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day4/declarative-manifests/lab1

oc apply -f mysql-deployment.yml
oc apply -f mysql-service.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>pwd</b>
/home/jegan/openshift-sep-2022-batch1/Day4/declarative-manifests/lab1
(jegan@tektutor.org)$ <b>ls</b>
mysql-deploy.yml  mysql-route.yml  mysql-service.yml
(jegan@tektutor.org)$ <b>oc apply -f mysql-deploy.yml</b>
deployment.apps/mysql created
(jegan@tektutor.org)$ <b>oc apply -f mysql-service.yml</b>
service/mysql created
</pre>

## Deleting mysql deployment in declarative sytle
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day4/declarative-manifests/lab1

oc delete -f mysql-deploy.yml
oc delete -f mysql-service.yml
```

## ⛹️‍♀️ Lab - Deploying mysql database server into OpenShift cluster with Persistent Volume
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day4/declarative-manifests/lab2

oc apply -f mysql-pv.yml
oc apply -f mysql-pvc.yml
oc apply -f mysql-deploy.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ oc get all
No resources found in jegan namespace.
(jegan@tektutor.org)$ ls
mysql-deploy.yml  mysql-pvc.yml  mysql-pv.yml  mysql-route.yml  mysql-service.yml
(jegan@tektutor.org)$ <boc apply -f mysql-pv.yml</b>
persistentvolume/mysql-pv-jegan created
(jegan@tektutor.org)$ <b>oc apply -f mysql-pvc.yml</b>
persistentvolumeclaim/mysql-pvc-jegan created
(jegan@tektutor.org)$ <b>oc apply -f mysql-deploy.yml</b>
deployment.apps/mysql created
(jegan@tektutor.org)$ oc get po -w
NAME                     READY   STATUS              RESTARTS   AGE
mysql-56b5b97485-qmdk6   0/1     ContainerCreating   0          4s
mysql-56b5b97485-qmdk6   1/1     Running             0          7s

</pre>

