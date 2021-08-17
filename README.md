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
To manage data you can you use WEB UI:
http://localhost:21000

Or work through REST API like:

```
$ curl -v -u admin:admin -X POST -H "Content-Type:application/json" -d '{"entity":{"typeName":"rdbms_table","attributes ":{"qualifiedName":"fyb.haha@192.168.2.11@mysql","createdBy":"root","name":"haha","comment":"rdbms_table API insert test","description": "rdbms_table manual input","owner":"root","type":"table","contact_info":"table_01_info","db":{"guid":"036b0662-cdb9-40ac-9ce5-d68d3670ab99" ,"typeName":"rdbms_db"}}}}' "http://localhost:21000/api/atlas/v2/entity"
```

This is default way to add data to base. Although it's better for you to configure access policy.


## Configurations

Atlas have simple form to manage data access that based on two files in ``distro/apache-atlas-${version}/conf`` directory.

The first one:
``atlas-simple-authz-policy.json``
is used to configure level of access via roles.

The second one: 
``users-credentials.properties``
stores users logins and passwords. Password is encrypted with SHA256. 

### More docs down below:

Apache Atlas docs: https://atlas.apache.org/#/

REST API docs: http://atlas.apache.org/api/v2/index.html
