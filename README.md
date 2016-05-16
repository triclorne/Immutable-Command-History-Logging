# Immutable-Command-History-Logging
I collected this the code about 6years ago from a source I cannot trace but I will find it to give credits.
It has a lot of bugs as it was originally meant for FreeBSD environment
Upon recoding a couple of lines to make it work, here is how to implement it on your Linux environment.
It helps you to keep an accurate track of every linux command that a user runs on a system and better yet 
make it immutable for forensics cases.
Even r00t user cannot edit the file unless a few ways to bypass it

Create a file in the directory /usr/local/bin/ 
git clone hcmnt into the folder /usr/local/bin/
Create a directory in the /var/log/ 

This will be the location to store all the command logs of users 
mkdir /var/log/histlog 

Set permissions to the various files 
chmod 644 /usr/local/bin/hcmnt 
chmod 711 /usr/bin/who ----out 
chmod -R 777  /var/log/histlog/ 

To enable the command logging for each user,there are two methods 



METHOD 1---A new server with no user on it 
vim /etc/skel/.bashrc 
and add the following at the end of the file 

source /usr/local/bin/hcmnt 
export hcmntextra='date "+%Y%m%d %R"' 
export PROMPT_COMMAND='hcmnt' 
export PROMPT_COMMAND='hcmnt -eityl /var/log/histlog/$LOGNAME $LOGNAME@$HOSTNAME' 

METHOD 2---A user already existed 

 Source the file by adding the below to .bashrc of users home 
source /usr/local/bin/hcmnt 
export hcmntextra='date "+%Y%m%d %R"' 
export PROMPT_COMMAND='hcmnt' 
export PROMPT_COMMAND='hcmnt -eityl /var/log/histlog/$LOGNAME $LOGNAME@$HOSTNAME' 


Security to make the log files open to the user alone and also immutable 
chattr -a /var/log/histlog/* 
chmod 620 /var/log/histlog/* 
chattr +a /var/log/histlog/* 

