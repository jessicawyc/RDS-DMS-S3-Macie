# Enabling data classification for Amazon RDS database with Macie 
将RDS导入S3后使用Macie检测将敏感数据分类分级Solution
updated code from aws security blog https://aws.amazon.com/blogs/security/enabling-data-classification-for-amazon-rds-database-with-amazon-macie/
原文中的cloudformation template运行报错,目前正在想办法修复中
## workaround
如果报错为IAM 创建dms-vpc-role失败,请使用模板:mysqlDms2s3-changerole.yml
如果Macie已开启的报错
> Resource handler returned message: "Resource of type 'AWS::Macie::Session' with identifier '###' already exists." (RequestToken: 1c2ca6aa-4149-b7e8-9c24-56ce05902794, HandlerErrorCode: AlreadyExists)
可以先关闭macie重试或者直接使用模板:mysqlDms2s3-changerole-nomacie.yml
Cloudformation完成后,再手动添加以下Policy给生成的IAM role :
```
wait
```
