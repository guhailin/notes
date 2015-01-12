使用linux上的cron工具来定时执行mysqldump来备份数据。
mysqldump -u hcm -phcm --database imdb | gzip > /home/hcm/my    sql_backup/imdb_`date +'\%Y-\%m-\%d-\%H-\%M'`.sql.gz