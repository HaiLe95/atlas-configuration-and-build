# atlas-configuration-and-build
Some documentation about how to build and how to configurate apache atlas.


## Requirements
JDK 8+

Python 2.7+

Maven 3.X

Git

## Build and Installation

Chose directory.

  ```
  $ cd <your-local-directory>  
  $ git clone https://github.com/apache/atlas.git
  $ cd atlas
  ```

Checkout the branch you would like to build.

  ```
  $ git checkout <branch>
  ```

Build.

  ```
  $ export MAVEN_OPTS="-Xms2g -Xmx4g"
  $ mvn clean -DskipTests install
  ```

If Solr and HBase are not installed.

  ```
  $ mvn clean -DskipTests package -Pdist,embedded-hbase-solr
  ```

There are many files are yet to build so take your time and wait 1-2 hours. Sometimes it can crush in the middle of process.
After build succeed there will be the new module in project structure named ``distro``. Inside the target package ``../distro/target`` you will find ``apache-atlas${version}-bin`` and ``apache-atlas${version}-server``.
Those files are needed to be unpacked.

Version depends on which branch you selected, so it's individual for you. In my case it was ``apache-atlas-3.0.0-SNAPSHOT-bin``.


Install Atlas.

  ```
  $ cd distro
  $ tar -xzvf target/apache-atlas-${version}-bin.tar.gz
  $ tar -xzvf target/apache-atlas-${version}-server.tar.gz
  ```

After finishing unpacking atlas and atlas-server there will be new package named same like ``apache-atlas-${version}`` in distro module.
This is the main directory we're working on.

## Start Atlas & Usage

  ```
  $ python apache-atlas-${version}/bin/atlas_start.py
  ```
To manage data you can use WEB UI:
http://localhost:21000

Or work through REST API like:

```
$ curl -v -u admin:admin -X POST -H "Content-Type:application/json" -d '{  "entity": {    "attributes": {      "owner": "admin",      "ownerName": "admin",      "name": "mysql instance",      "qualifiedName": "mysql_instance@ cluster_test2",      "rdbms_type": "mysql",      "description": "mysql instance description",      "contact_info": "your contact info",      "platform": "Linux",      "hostname": "mysql.hostname.com",      "protocol": "mysql protocal",      "port": "3306"    },    "typeName": "rdbms_instance",    "status": "ACTIVE"  }}' "http://localhost:21000/api/atlas/v2/entity"
```

This is default way to add data to base. Although it's better for you to configure access policy.
Apache Atlas using NoSQL HBase as default database to store metadata.

To stop Apache Atlas:
```
$ python apache-atlas-${version}/bin/atlas_stop.py
```

## Configurations

Atlas have simple form to manage data access that based on two files in ``distro/apache-atlas-${version}/conf`` directory.

The first one:
``atlas-simple-authz-policy.json``
is used to configure level of access via roles.

The second one: 
``users-credentials.properties``
stores users logins and passwords. Password is encrypted with SHA256. 

SHA256 hashtool: https://emn178.github.io/online-tools/sha256.html

### More docs down below:

Apache Atlas docs: https://atlas.apache.org/#/

REST API docs: http://atlas.apache.org/api/v2/index.html
