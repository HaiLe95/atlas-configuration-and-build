# atlas-configuration-and-build
Some documentation about how to build and how to configuration apache atlas.


## Requirements
JDK 8+
Python 2.7+
Maven 3.X
Git

## Build and Installation

Chose directory.
  ``
  $ cd <your-local-directory>  
  $ git clone https://github.com/apache/atlas.git
  $ cd atlas
  ``
  
Checkout the branch or tag you would like to build.
  
  $ git checkout <branch>
     
Build.
  
  $ export MAVEN_OPTS="-Xms2g -Xmx4g"
  $ mvn clean -DskipTests install
  
If Solr and HBase are not installed.
  
  $ mvn clean -DskipTests package -Pdist,embedded-hbase-solr

There are much files are yet to build so take your time and wait 1-2 hours. Sometimes it crushes in the middle of proccesses.
After build succeed there will be new module in project structure named distro. Inside the target package [../distro/target] you will find [apache-atlas${version}-bin] and [apache-atlas${version}-server].
Those files are needed to be unpacked.

Install Atlas.
  
  $ tar -xzvf apache-atlas-${version}-bin.tar.gz
  $ tar -xzvf apache-atlas-${version}-server.tar.gz
  
After finishing installization. There're will be new package named same like [apache-atlas-${version}] in my case it is [apache-atlas-3.0.0-SNAPSHOT].
This is main directory we're working on. 

Let's start a server.
  
  $ python distro/apache-atlas-${version}/bin/atlas_start.py
  
  
URL
  http://localhost:21000
