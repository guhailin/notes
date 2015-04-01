使用linux上的cron工具来定时执行mysqldump来备份数据。
mysqldump -u hcm -phcm --database imdb | gzip > /home/hcm/my    sql_backup/imdb_`date +'\%Y-\%m-\%d-\%H-\%M'`.sql.gz

mysql权限
GRANT ALL PRIVILEGES ON *.* TO monty@`%` IDENTIFIED BY 'something' WITH GRANT OPTION; 

mysql重置root密码
1. 新建mysql-init.txt文件，内容如下

UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
FLUSH PRIVILEGES;

2. 运行mysqld --init-file=mysql-init.txt
