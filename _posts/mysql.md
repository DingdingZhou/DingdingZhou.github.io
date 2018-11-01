## 导出整个数据库结构和数据
mysqldump -h localhost -uroot -p123456 database > dump.sql

## 导出单个数据表结构和数据
mysqldump -h localhost -uroot -p123456  database table > dump.sql

## 导出整个数据库结构（不包含数据）
mysqldump -h localhost -uroot -p123456  -d database > dump.sql

## 导出单个数据表结构（不包含数据）
mysqldump -h localhost -uroot -p123456  -d database table > dump.sql
