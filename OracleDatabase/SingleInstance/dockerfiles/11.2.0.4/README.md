构建oracle database 11.2.0.4镜像所需文件
参考：
https://github.com/54554980/oracle11g-11.2.0.4-ee
https://github.com/oracle/docker-images/tree/master/OracleDatabase/SingleInstance


构建方法：
1. 将p13390677_112040_Linux-x86-64_1of7.zip和p13390677_112040_Linux-x86-64_2of7.zip放在当前目录下
2. 执行脚本构建镜像
./buildDockerImage.sh -e -v 11.2.0.4
3. 运行容器
docker run -p 1521:1521 -p 8080:8080 -e ORACLE_SID=mysid -e ORACLE_PWD=mypwd -e ORACLE_CHARACTERSET=UTF-8 oracle/database:11.2.0.4-ee


使用docker-compose运行请参考docker-compose.yml
使用k8s运行请参考oracle-db-11.2.0.4.yaml