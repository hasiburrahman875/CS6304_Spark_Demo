# Spark

### General Information:

* Operating System:         Mac -> Microsoft Remote Desktop, Windows -> Default Remote Desktop, Ubuntu -> Remmina
* Machine:                  cs6304-<mst_username>-01.class.mst.edu
* User:                     <mst_username>
* Default Password:         <mst_password>



### Spark Demo
Follow the instructions from commands.txt file.




### Install Spark (TO STUDENTS: DO NOT TRY TO INSTALL)
DO NOT Install Spark again. It is already installed in your VM.  
You can consider the below section as a self note.  
We consider that Java and Hadoop are already installed.\\
Follow the below steps:
1. Open a terminal and enter below commands
```
sudo apt update
```
```
wget https://dlcdn.apache.org/spark/spark-3.2.2/spark-3.2.2-bin-hadoop3.2.tgz
tar xvf spark-3.2.2-bin-hadoop3.2.tgz
rm spark-3.2.2-bin-hadoop3.2.tgz
sudo mv spark-3.2.2-bin-hadoop3.2/ /opt/spark 
```
```
sudo tee /etc/profile.d/spark.sh <<'EOF'
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
EOF
```
2. Go to top right corner and select power button -> power off
3. Log in to remote desktop again. Now your spark path is set and you can type "spark-shell" in terminal to access spark.\\
You can access spark by typing absolute path "/opt/spark/bin/spark-shell" also.