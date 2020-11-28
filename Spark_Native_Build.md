# 在hadoop叢集上建立spark

### 安裝檔版本
1.spark-2.4.5-bin-hadoop2.7.tgz

2.Anaconda3-2020.02-Linux-x86_64.sh

### 安裝流程
1. 下載並安裝Anaconda3(每台都要):
https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh -> `./Anaconda3-2020.02-Linux-x86_64.sh`

2.  修改權限
  ```js
  chmod +x Anaconda3-2020.02-Linux-x86_64.sh
  ```

3. 同意所有條款，然後vim ~/.bashrc，加上:(設定路徑)  
```js
export ANACONDA_HOME=/home/{username}/anaconda3
export PATH=$ANACONDA_HOME/bin:$PATH
```  
4. surce and test  


5. 下載spark-2.4.5-bin-hadoop2.7.tgz

6. 解壓縮:
  ```js
  cd ~/Downloads
  tar zxvf spark-2.4.5-bin-hadoop2.7.tgz 
  mv spark-2.4.5-bin-hadoop2.7 ~/
  ```
  
7. 設定`.bashrc`環境變數SPARK_HOME 跟 PATH:  
  在最底下加上以下全部:(記得`source ~/.bashrc`)
  ```js
  export SPARK_HOME=/home/{username}/spark-2.4.5-bin-hadoop2.7  #注意路徑
  export PATH=$SPARK_HOME/bin:$PATH
  export PYSPARK_PYTHON=python

  # use ipython as the interactive shell
  export PYSPARK_DRIVER_PYTHON=ipython

  # use jupyter as the interactive shell
  #export PYSPARK_DRIVER_PYTHON=jupyter    # 選擇使用jupyter notebook開啟
  #export PYSPARK_DRIVER_PYTHON_OPTS=notebook
  ```
  
8. 設定spark-env.sh:
  ```js
  cd ~
  cd spark-2.4.5-bin-hadoop2.7/conf/
  mv spark-env.sh.template spark-env.sh
  vim spark-env.sh
  ```
  在最底下加上以下全部:
  ```js
  export JAVA_HOME=/home/{username}/jdk1.8.0_251
  export PATH=$JAVA_HOME/bin:$PATH

  export HADOOP_HOME=/home/{username}/hadoop-2.10.0
  export PATH=$HADOOP_HOME/bin:$PATH

  export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop

  export SCALA_HOME=/home/{username}/scala-2.12.11
  export PATH=$SCALA_HOME/bin:$PATH

  export ANACONDA_HOME=/home/{username}/anaconda3
  export PATH=$ANACONDA_HOME/bin:$PATH

  export SPARK_HOME=/home/{username}/spark-2.4.5-bin-hadoop2.7
  export PATH=$SPARK_HOME/bin:$PATH
  ```
  
9. 設定 slaves:  
  ```js
  mv slaves.template slaves
  vim slaves
  ```
  把"localhost"改成:
  ```js
  {master}  #master的主機名稱
  {slave1}  #slave1的主機名稱
  {slave2}
  ```
  
10.scp整個spark 資料夾與/.bashrc檔

  
11. 開啟spark:
  ```js
  cd ~/spark-2.4.5-bin-hadoop2.7/sbin
  ./start-all.sh
  ```
12. 觀察Spark Standalone 狀態:
   網址:http://{hostname}:8080

13. 測試/執行
   ```js
   pyspark --master spark://{hostname}:7077
   ```  
   
* 註:只顯示ERROR 等級以上的提示
  * 到Spark目錄內的conf目錄找到`log4j.properties.template`
  ```js
  cp log4j.properties.template log4j.properties   #複製一個出來
  ```
  * 編輯:找到`log4j.rootCategory=INFO, console`，把 `INFO` 改成`ERROR`


