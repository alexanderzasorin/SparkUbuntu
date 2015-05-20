Apache Spark Installation + ipython notebook integration guide for Ubuntu 14.04
===================

verified on jdk 1.8.0_45-b14

Install Java Development Kit 8
--------------------
The following directions are from [here](http://tecadmin.net/install-oracle-java-8-jdk-8-ubuntu-via-ppa/)
```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```

Add following code to your e.g. `.bash_profile`
```bash
# For Apache Spark
JAVA_HOME=/usr/lib/jvm/java-8-oracle
```

Install Apache Spark
--------------------
use [download instructions](https://spark.apache.org/downloads.html) to download spark
```bash
sudo su
cd /usr/local/src
git clone git://github.com/apache/spark.git -b branch-1.3
```
use [install instructions](https://spark.apache.org/documentation.html) to install spark
```bash
/usr/local/src/spark/make-distribution.sh --skip-java-test
```
----------
Set up env variables
--------------------
Add following code to your e.g. `.bash_profile`
Don't forget return from root to the user you need to configure ipython notebook
```bash
# For a ipython notebook and pyspark integration
export SPARK_HOME="/usr/local/src/spark"
export PYSPARK_SUBMIT_ARGS="--master local[2]"
```

Create ipython profile
----------------------
Run
```bash
ipython profile create pyspark
```

Create a startup file
```
$ vim ~/.ipython/profile_pyspark/startup/00-pyspark-setup.py
```
```python
# Configure the necessary Spark environment
import os
import sys

spark_home = os.environ.get('SPARK_HOME', None)
sys.path.insert(0, spark_home + "/python")

# Add the py4j to the path.
# You may need to change the version number to match your install
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.8.2.1-src.zip'))

# Initialize PySpark to predefine the SparkContext variable 'sc'
execfile(os.path.join(spark_home, 'python/pyspark/shell.py'))
```

Run ipython
-----------
```
ipython notebook --profile=pyspark
```

`sc` variable should be available
```ipython
In [1]: sc
Out[1]: <pyspark.context.SparkContext at 0x10a982b10>
```

