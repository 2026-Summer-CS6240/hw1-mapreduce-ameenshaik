[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9ztpjGjb)
Hadoop MapReduce WordCount Demo
Example code for CS6240

Author
-----------
- Joe Sackett (2018)
- Updated by Nikos Tziavelis (2023)
- Updated by Mirek Riedewald (2024)
- Updated by Diego Rivera Correa (2026)
- Updated by Ameen Shaik (2026)

Installation
------------
These components need to be installed first:
- OpenJDK 11
- Hadoop 3.3.5
- Maven (Tested with version 3.9.15)
- AWS CLI (Tested with v1, installed via pipx)

After downloading the hadoop installation, move it to an appropriate directory:
`mv hadoop-3.3.5 /usr/local/hadoop`

Environment
-----------
1) Example ~/.bashrc entries (for ARM64/Apple Silicon Ubuntu VM):
# Java
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64

# Hadoop
export HADOOP_HOME=/usr/local/hadoop

# Maven
export MAVEN_HOME=/usr/local/maven

# Scala
export SCALA_HOME=/usr/local/scala

# Spark
export SPARK_HOME=/usr/local/spark
export SPARK_DIST_CLASSPATH=$(hadoop classpath)

# All paths (set SPARK_DIST_CLASSPATH after this line)
export PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$MAVEN_HOME/bin:$SCALA_HOME/bin:$SPARK_HOME/bin:$HOME/.local/bin:$PATH

export SPARK_DIST_CLASSPATH=$(hadoop classpath)
2) Explicitly set `JAVA_HOME` in `$HADOOP_HOME/etc/hadoop/hadoop-env.sh`:
	`export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64`

3) Set standalone mode before first local run:
	`make switch-standalone`

Makefile Configuration
-----------
Edit the following variables at the top of the Makefile before running:
hadoop.root=/usr/local/hadoop
jar.name=mr-demo-1.0.jar
local.input=input
local.output=output
aws.bucket.name=cs6240-ameenshaik-bucket
aws.region=us-east-1
aws.instance.type=m4.large
aws.core.num.nodes=2
aws.primary.num.nodes=1
Execution
---------
All build and execution commands are organized in the Makefile.

1) Clone the repository:
	`git clone git@github.com:2026-Summer-CS6240/hw1-mapreduce-ameenshaik.git`

2) Navigate to the project directory:
	`cd hw1-mapreduce-ameenshaik`

3) Edit the Makefile to customize the variables at the top.

4) Standalone Local Hadoop:
	- `make switch-standalone`	-- set standalone Hadoop environment (execute once)
	- `make local`			-- build jar and run Word Count locally

5) Pseudo-Distributed Hadoop:
	- `make switch-pseudo`		-- set pseudo-clustered Hadoop environment (execute once)
	- `make pseudo`			-- first execution
	- `make pseudoq`		-- later executions since namenode and datanode already running

6) AWS EMR Hadoop:
	- Configure AWS CLI first:
		- Install via pipx: `pipx install awscli`
		- Run `aws configure` and enter your credentials and region (us-east-1)
		- Or manually edit `~/.aws/credentials` with your Learner Lab credentials
	- `make make-bucket`		-- create S3 bucket (only before first execution)
	- `make upload-input-aws`	-- upload input data to S3 (only before first execution)
	- `make aws`			-- build jar, upload to S3, and launch EMR cluster
	- `make download-output-aws`	-- download output from S3 after successful execution

Notes
-----------
- AWS Learner Lab credentials expire when the session ends. Refresh them in
  `~/.aws/credentials` before each AWS run.
- The EMR cluster uses `--auto-terminate` so it shuts down automatically after
  the job completes.
- Make sure the output directory does not exist before running locally,
  or use `make clean-local-output` first.
  