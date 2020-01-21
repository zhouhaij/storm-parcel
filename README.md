# 制作Apache Storm的parcel包
# 安装 CSD validator
```
cd /opt
mkdir git-cm-ext
git clone https://github.com/cloudera/cm_ext
cd cm_ext/validator
mvn install
```

#下载 Apache Storm
```
cd /opt

打开 http://storm.apache.org
copy link to binary you want to download (with tar.gz extemsion) 
in my case it is: https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.0.0.tar.gz
wget -O apache-storm-2.1.0.tar.gz "https://www.apache.org/dyn/closer.lua/storm/apache-storm-2.1.0/apache-storm-2.1.0.tar.gz"
```
#制作 parcel and CSD
#执行以下命令
```
POINT_VERSION=5 VALIDATOR_DIR=/opt/git-cm-ext/cm_ext OS_VER=el7 PARCEL_NAME=STORM ./build-parcel.sh /opt/apache-storm-1.0.0.tar.gz
VALIDATOR_DIR=/opt/git-cm-ext/cm_ext CSD_NAME=STORM ./build-csd.sh
python /opt/git-cm-ext/cm_ext/make_manifest/make_manifest.py /opt/git-cm-ext/storm-parcel/build-parcel
```

#创建.parcel.sha文件
```
cd build-parcel
cat manifest.json
# 查看json中的hash码
# 将此hash码写入STORM-xxx.storm.p0.5-el7.parcel.sha文件中
```

#拷贝 parcel文件至 Cloudera Manager's CSD Repo
```
cd ../
cp build-csd/STORM-1.0.jar /opt/cloudera/csd
cp /opt/git-cm-ext/storm-parcel/build-parcel/STORM-* /opt/cloudera/parcel-repo/
service cloudera-scm-server restart
# Wait a min, go to Cloudera Manager -> Add a Service -> Storm
