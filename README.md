# Spark

### VM Access Instructions

1. **Network Requirement**

   * If you are on the **S\&T network**, you do **not** need a VPN.
   * If you are **off-campus**, please install and connect to the VPN before accessing.
   * [S\&T VPN Installation Guide](https://it.mst.edu/services/vpn/)

2. **Remote Desktop Applications**

   * **Mac:** Use the **Microsoft Remote Desktop app (currently it is Windows APP)**.
   * **Windows:** Use the **built-in Remote Desktop** application.
   * **Ubuntu/Linux:** Use **Remmina**.

3. **Connection Details**

   * **Machine:** `cs6304-<mst_username>-01.class.mst.edu`
   * **Username:** `<mst_username>`
   * **Default Password:** `<mst_password>`
     
## Do not shutdown or, restart, or sign out, or user switch from the VM

## Fixing Black Screen Issue on Remote Desktop (XRDP)

````markdown

If you encounter a black screen issue when connecting to the VM via Remote Desktop, follow the steps below:

SSH into the VM
```
ssh <username>@cs6304-<username>-01.class.mst.edu
````

Update and Install Required Packages

```bash
sudo apt update
sudo apt install -y xorgxrdp xfce4 xfce4-goodies dbus-x11
```

Configure Session

```bash
echo "startxfce4" > ~/.xsession
```

Refresh Group Membership

```bash
newgrp ssl-cert
```

Remove Old Session Files

```bash
rm -f ~/.Xauthority ~/.xsession-errors 2>/dev/null || true
```

Restart XRDP Services

```bash
sudo systemctl restart xrdp xrdp-sesman
```

Verify XRDP Status

```bash
sudo systemctl status xrdp xrdp-sesman --no-pager
```

### Test Hadoop Accessibility:
Run the following commands in your VM terminal:
```
stop-all.sh
hadoop namenode -format
start-all.sh
```
Run the following command to check the services:
```
jps
```
If everything is fine, you should see the following jobs (ignore the job number in the left) running
```
13623 NameNode
14088 SecondaryNameNode
14297 ResourceManager
49546 Jps
14459 NodeManager
13807 DataNode
```

##### if DataNode is Missing:

**Problem:** NameNode UI (`http://localhost:9870`) shows **no DataNodes**.

**Fix:**

```bash
start-dfs.sh
start-yarn.sh
```

If still missing, clear old DataNode data:

```bash
rm -r /tmp/hadoop-dtg95/dfs/data/
stop-all.sh
hadoop namenode -format
start-all.sh
```



### Spark Demo
Follow the instructions from commands.txt file.




### Install Spark
DO NOT Install Spark again if it is already installed in your VM.  
You can consider the below section as a self-note.  
We consider that Java and Hadoop are already installed.
Follow the below steps:
1. Open a terminal and enter the below commands
```
sudo apt update
```
```
wget https://mirror.lyrahosting.com/apache/spark/spark-3.3.3/spark-3.3.3-bin-hadoop3.tgz
tar xvf spark-3.3.3-bin-hadoop3.tgz
rm spark-3.3.3-bin-hadoop3.tgz
sudo mv spark-3.3.3-bin-hadoop3/ /opt/spark 
```
```
sudo tee /etc/profile.d/spark.sh <<'EOF'
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
EOF
```
2. Go to the top right corner and select the power button -> Log Out [Don't power off]
3. Log in to the remote desktop again. Now your spark path is set and you can type "spark-shell" in the terminal to access spark.
You can access spark by typing absolute path "/opt/spark/bin/spark-shell" also.
4. If you see following error:
   ```
   Error: A JNI error has occurred, please check your installation and try again Exception in thread "main" java.lang.UnsupportedClassVersionError:
   ```
  Do:

##### Step 1: Install Java 17 (without removing Java 8)

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

Now you’ll have both Java 8 and Java 17.


##### Step 2: Point Spark to Java 17 only

Instead of changing the whole system’s Java, just tell Spark which Java to use by setting the `JAVA_HOME` variable.

Edit your `~/.bashrc` (or `~/.zshrc` if using zsh):

```bash
nano ~/.bashrc
```

Add this at the end:

```bash
# Spark should use Java 17
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

Save and reload:

```bash
source ~/.bashrc
```


##### Step 3: Verify Spark works with Java 17

Check Java inside your shell:

```bash
java -version
```

It should now show something like:

```
openjdk version "17.0.x"
```

Then run Spark:

```bash
spark-shell
```

You should see the Spark logo and Scala REPL prompt (`scala>`).


### Run a scala file
Run a scala file using the following command 
```
spark-shell
:load SimpleWordCount.scala
```
Please replace the paths according to your file locations. If you are facing issues please make sure you are giving the correct paths while loading the Hadoop file from the HDFS
