# Enabling data classification for Amazon RDS database with Macie 
将RDS导入S3后使用Macie检测将敏感数据分类分级Solution
updated code from aws security blog https://aws.amazon.com/blogs/security/enabling-data-classification-for-amazon-rds-database-with-amazon-macie/
原文中的cloudformation template运行报错,已于今年2022年5月22日完成修复,可以直接使用.
以下workaround不再需要
--------------------------------------------------------------------------------------------------------------------------------------------------------
## Workaround
如果报错为IAM 创建dms-vpc-role失败,
>The IAM Role arn:aws:iam::*:role/dms-vpc-role is not configured properly. (Service: AWSDatabaseMigrationService; Status Code: 400; Error Code: AccessDeniedFault; Request ID: ad9a719b-231d-4c0b-9767-a735845debb9; Proxy: null)
>
请使用模板:[mysqlDms2s3-changerole.yml](https://github.com/jessicawyc/RDS-DMS-S3-Macie/blob/main/mysqlDms2s3-changerole.yml)

如果Macie已开启的报错
> Resource handler returned message: "Resource of type 'AWS::Macie::Session' with identifier '###' already exists." (RequestToken: 1c2ca6aa-4149-b7e8-9c24-56ce05902794, HandlerErrorCode: AlreadyExists)
> 
可以先关闭macie重试或者直接使用模板:[mysqlDms2s3-changerole-nomacie.yml](https://github.com/jessicawyc/RDS-DMS-S3-Macie/blob/main/mysqlDms2s3-changerole-nomacie.yml)

Cloudformation完成后,需要手动添加以下IAM Policy给生成的dms-vpc-role :
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DMStoS3Policy",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:ListBucket",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::dcp-macie-<bucketname>",
                "arn:aws:s3:::dcp-macie-<bucketname>/*"
            ]
        }
    ]
}
```
