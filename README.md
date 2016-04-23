# 目的
AWS CloudFormationでVPCを管理できるようにする

# 前提
| ソフトウェア     | バージョン    | 備考         |
|:---------------|:-------------|:------------|
| docker         | 1.10.3       |             |
| docker-compose | 1.6.2        |             |
| vagrant        | 1.7.4        |             |
| aws-cli        | 1.10.17      |             |
| EB CLI         | 3.7.4        |             |

+ AWSのアカウントを作っている

# 構成

+ 準備
+ ハンズオン
+ 片付け

# 詳細
## 準備
```
$ vagrant up
$ vagrant ssh
$ cd /vagrant/
$ aws configure
AWS Access Key ID [None]: XXXXXXXXXXXXXXXXXX
AWS Secret Access Key [None]: XXXXXXXXXXXXXXXXXX
Default region name [None]: ap-northeast-1
Default output format [None]: json
```

## ハンズオン
### １リージョン１アベイラビリティゾーン１パブリックネットワークを作成する
```
$ aws cloudformation validate-template --template-body file://vpc-1az-1subnet-pub.template
{
    "Description": "AWS CloudFormation Sample Template.", 
    "Parameters": []
}
$ aws cloudformation create-stack --stack-name Sample --template-body file://vpc-1az-1subnet-pub.template --tags Key=Name,Value=test
{
    "StackId": "arn:aws:cloudformation:ap-northeast-1:262470114399:stack/Sample/afce9230-08f1-11e6-8a93-50d5ca9ff48e"
}
```
### １リージョン１アベイラビリティゾーン２パブリックネットワークを作成する
```
$ aws cloudformation validate-template --template-body file://vpc-1az-2subnet-pub.template
$ aws cloudformation delete-stack --stack-name Sample
$ aws cloudformation create-stack --stack-name Sample --template-body file://vpc-1az-2subnet-pub.template --tags Key=Name,Value=test
```
### １リージョン１アベイラビリティゾーン１パブリック１プライベートネットワークを作成する
```
$ aws cloudformation validate-template --template-body file://vpc-1az-2subnet-pub-priv.template
$ aws cloudformation delete-stack --stack-name Sample
$ aws cloudformation create-stack --stack-name Sample --template-body file://vpc-1az-2subnet-pub-priv.template --tags Key=Name,Value=test
```
### １リージョン２アベイラビリティゾーン２パブリックネットワークを作成する
```
$ aws cloudformation validate-template --template-body file://vpc-2az-2subnet-pub.template
$ aws cloudformation delete-stack --stack-name Sample
$ aws cloudformation create-stack --stack-name Sample --template-body file://vpc-2az-2subnet-pub.template --tags Key=Name,Value=test
```
### １リージョン２アベイラビリティゾーン２パブリック２プライベートネットワークを作成する
```
$ aws cloudformation validate-template --template-body file://vpc-2az-4subnet-pub-priv.template
$ aws cloudformation delete-stack --stack-name Sample
$ aws cloudformation create-stack --stack-name Sample --template-body file://vpc-2az-4subnet-pub-priv.template --tags Key=Name,Value=test
```
## 片付け
```
$ aws cloudformation delete-stack --stack-name Sample
$ exit
$ vagrant destroy
```

# 参照
+ [AWS CloudFormatin](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/best-practices.html)
+ [AWS CLI CloudFormation](http://docs.aws.amazon.com/cli/latest/reference/cloudformation/)

