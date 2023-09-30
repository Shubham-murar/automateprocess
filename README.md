# automateprocess
//these are the commands i have used ec2 instances, we have to put these commands serial wise, then only ./mongobackup.sh will work

1) sudo su
2) yum update -y

Step 2:
 Installing MongoDB and Tools:

3) sudo vi /etc/yum.repos.d/mongodb-org-4.4.repo

4) [mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2...
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/
server-4.4.asc

5)
sudo yum install -y mongodb-org

6)
sudo systemctl start mongod

7)
sudo systemctl status mongod

8)- to go mongo terminal 
mongo

9)
show dbs

10)
use mybackupDB (name of your Database file in mongodb)

11)
db.createCollection("employees") 

12)
exit
13)Create Directory
mkdir DBbackups

14)create .sh file backup script

nano mongobackup.sh

15)paste

folder_name=”/home/synonymous/DBbackups/”                        //replace 'synonymous' with the name of your Database
file=”backupStorage”
now=$(date +”%b%d-%Y-%H%M%S”)
ext=”.gzip”
file_name=$file$now$ext
backupPath=$folder_name$file_name
echo $backupPath

mongodump  --db=mybackupDB  --archive=$backupPath  --gzip

aws s3 cp $backupPath  s3://s3mongobucket/backup/
rm $backupPath

16)
chmod  +x mongobackup.sh

17)execute sh file
./mongobackup.sh

18)cron schedular
crontab -e
* * * * * /home/ec2-user/mongobackup.sh       //use 'https://crontab.guru/' to replace actual numbers with * 
