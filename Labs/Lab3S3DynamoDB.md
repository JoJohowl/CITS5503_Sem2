# Practical Worksheet 3

Version: 1.0 Date: 12/04/2018 Author: David Glance

Date: 24/07/2024 Updated by Zhi Zhang

## Learning Objectives

1. Learn how to create and configure S3 buckets and read and write objects to them
2. Learn how to use operations on DynamoDB: Create table, put items, get items
3. Start an application as your own personal Cloud Storage

## Technologies Covered

* Ubuntu
* AWS
* AWS S3
* AWS DynamoDB
* Python/Boto scripts
* VirtualBox

**NOTE**: please use your Linux environment – if you do it from any other OS (e.g., Windows, Mac – some unknow issues might occur)

## Background

The aim of this lab is to write a program that will:

[1] Scan a directory and upload all of the files found in a directory to an S3 bucket, preserving the path information

[2] Store information about each file uploaded to S3 in a DynamoDB

[3] Restore the directory on a local drive using the files in S3 and the information in DynamoDB

## Program

### [1] Preparation

Download the python code `cloudstorage.py` from the directory of [src](https://github.com/zhangzhics/CITS5503_Sem2_2023/blob/master/Labs/src/cloudstorage.py) \
Create a directory `rootdir` \
Create a file in `rootdir` called `rootfile.txt` and write some content in it `1\n2\n3\n4\n5\n` \
Create a second directory in rootdir called `subdir` and create another file `subfile.txt` with the same content as `rootfile.txt`.

### [2] Save to S3 by updating `cloudstorage.py`

Create an S3 bucket named `<student ID>-cloudstorage`

When the program traverses the directory starting at the root directory `rootdir`, upload each file onto the S3 bucket. An easy way to upload files is to use the command below:

```
s3.upload_file()
```

**NOTE**: Make sure your S3 bucket has the same file structure as shown in `[1] Preparation`.

### [3] Restore from S3

Create a new program called `restorefromcloud.py` that reads the S3 bucket and writes the contents of the bucket within the appropriate directories. 

**NOTE**: Your local Linux environment should see a copy of the files and the directories from the S3 bucket.

### [4] Write information about files to DynamoDB

Install DynamoDB on your Linux environment

```
mkdir dynamodb
cd dynamodb
```

Install jre if not done

```
sudo apt-get install default-jre
wget https://s3-ap-northeast-1.amazonaws.com/dynamodb-local-tokyo/dynamodb_local_latest.tar.gz
java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar –sharedDb
```

Alternatively, you can use docker in Week 2:
```
docker run -p 8000:8000 amazon/dynamodb-local -jar DynamoDBLocal.jar -inMemory -sharedDb
```

Write a Python script to create a table on your local DynamoDB with the key `userId` and the attributes for the table are:

```
        CloudFiles = {
            'userId',
            'fileName',
            'path',
            'lastUpdated',
	    'owner',
            'permissions'
            }
        )
```

Inside the script, retrieve the file information of every file that is stored in the S3 bucket, and write them into the table. You should find functions in Python to get the file information including  `lastUpdated`, `owner` and `permissions`. All the information can be stored as strings.

Lab Assessment:

A structured presentation (15%). A clear step-by-step with detailed descriptions (85%). 
