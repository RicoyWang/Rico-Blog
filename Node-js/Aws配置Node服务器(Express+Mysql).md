### ```pm2 管理部署后的 node服务```
  ``` 
  pm2 start app.js#我使用的是express部署，他得启动命令是pm2 start /bin/www
  pm2 start app.js -i  4 # cluster mode 模式启动4个app.js的应用实例
                         # 4个应用程序会自动进行负载均衡
  npm install -g pm2 命令行全局安装pm2
  pm2 start app.js 启动app项目
  pm2 list 列出由pm2管理的所有进程信息，还会显示一个进程会被启动多少次，因为没处理的异常。
  pm2 reload all 0秒停机重载进程 (用于 NETWORKED 进程)
  pm2 stop all 停止所有进程
  pm2 restart all 重启所有进程
  pm2 restart 0 重启指定的进程
```
### ```安装 mysql```
```e
yum 安装mysql
1. 查看是否安装有mysql:yum list installed mysql*, 如果有先卸载掉, 然后在进行安装;
2. 安装mysql客户端:yum -y install mysql;
3. 安装mysql服务器端 :yum -y install mysql_server;
4. 安装mysql开发库 :yum -y install mysql-devel;
5. 配置mysql配置文件 : 设置utf-8编码 :vim /etc/my.cnf , 添加default-character-set=utf8;
6. 启动mysql数据库 :service mysqld start;
7. 创建root密码 : mysqladmin -u root password *****;
8. 进入数据库:mysql -u root -p 之后提示输入密码, 输入密码后进入;
9. 使用mysql数据库 :>use mysql 
10. 退出mysql :>\q;
```
  - ```yum```一直提示```timeout```：在安装mysql时yum下载包一直提示连接超时，并重试。解决：安全组的出站规则被限制，可以设为所有TCP。
  - 导入```.sql```
    ```mysql -u资料库用户 -p 资料库名 < /home/root/文件名.sql
      主要是增加一下存放sql文件的目录
      mysql -uroot -p dbname < /home/root/filename.sql
      ```
```
mysql> use mysql
ERROR 1044 (42000): Access denied for user 'root'@'localhost' to database 'mysql'
mysql> exit
Bye
[root@testtest ~]# service mysqld stop 
Stopping mysqld:                      [ OK ]
[root@testtest ~]# mysqld_safe --user=mysql --skip-grant-tables --skip-networking & 

[root@testtest ~]# mysql -u root -p -hlocalhost
Enter password: 

mysql> use mysql

mysql> SELECT host,user,password,Grant_priv,Super_priv FROM mysql.user;

mysql> UPDATE mysql.user SET Grant_priv='Y', Super_priv='Y' WHERE User='root';

mysql> FLUSH PRIVILEGES;

mysql> GRANT ALL ON *.* TO 'root'@'localhost';

mysql> GRANT ALL ON *.* TO 'root'@'cn.cn.cn.cn';

mysql> GRANT ALL ON *.* TO 'root'@'245.245.245.245';

mysql> GRANT ALL ON *.* TO 'root'@'127.0.0.1';

mysql> FLUSH PRIVILEGES;


mysql> quit
Bye
[root@testtest ~]# service mysqld start 

restart Linux/OS 
```
