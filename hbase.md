```shell
tar -zxvf hbase-2.4.11-bin.tar.gz -C /opt/module/
mv hbase-2.4.11/ hbase
sudo vim /etc/profile.d/my_env.sh

#HBASE_HOME
export HBASE_HOME=/opt/module/hbase
export PATH=$PATH:$HBASE_HOME/bin

sudo /home/wuss/bin/xsync /etc/profile.d/my_env.sh
source /etc/profile.d/my_env.sh
```
## 1. 修改配置
```shell
cd /opt/module/hbase/conf
vim hbase-env.sh

找到修改为false
export HBASE_MANAGES_ZK=false


vim hbase-site.xml
============================================================================
<configuration>

 <property>
 <name>hbase.zookeeper.quorum</name>
 <value>hadoop102,hadoop103,hadoop104</value>
 <description>The directory shared by RegionServers.
 </description>
 </property>
<!-- <property>-->
<!-- <name>hbase.zookeeper.property.dataDir</name>-->
<!-- <value>/export/zookeeper</value>-->
<!-- <description> 记得修改 ZK 的配置文件 -->
<!-- ZK 的信息不能保存到临时文件夹-->
<!-- </description>-->
<!-- </property>-->

 <property>
 <name>hbase.rootdir</name>
 <value>hdfs://hadoop102:8020/hbase</value>
 <description>The directory shared by RegionServers.
 </description>
 </property>
 
 <property>
 <name>hbase.cluster.distributed</name>
 <value>true</value>
 </property>
</configuration>
============================================================================
vim regionservers
去掉localhost添加
hadoop102
hadoop103
hadoop104

mv /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar /opt/module/hbase/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar.bak
cd /opt/module
xsync hbase/
```
## 2. 启动
```shell
群起
start-hbase.sh
关闭
stop-hbase.sh

可在浏览器hadoop102:16010页面查看
```
## 3. 设置高可用
```shell
stop-hbase.sh
cd /opt/module/hbase/conf
vim backup-masters
写入hadoop103
xsync backup-masters

start-hbase.sh
此时可在web页面看到Backup Masters里多了hadoop103
可前往hadoop103:16010查看
```