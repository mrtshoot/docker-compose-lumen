# Docker Compose for Lumen Infrastructure
this repository provide lumen infrastructure with docker compose orchestrator

### Step1
go to /opt and clone this repository
```
cd /opt;git clone http://git.mobtaker.local/mobtaker-team/docker-compose-lumen.git
```

### Step2
change your directory to cloned directory and cp your env files for php and db and dblogging images and change it to your own information.
```
cp .env/.db.env.example .env/db.env
cp .env/.dblogging.env.example .env/dblogging.env
cp .env/.php.env.example .env/php.env
```

### Step3
cp your local.ini file for php configuration files and add any setting you have like.
```
cp php/.local.ini.example php/local.ini
```

### Step4
cp your app.conf nginx configuration file for server proxy and add your setting.
```
cp nginx/conf.d/.app.conf.example nginx/conf.d/app.conf
```
### Step5
cp your mysql configuration file
```
cp mysql/.my.cnf.example mysql/my.cnf
cp mysql/.mydblogging.cnf.example mysql/mydblogging.cnf
```

### Step6
set your project name to configure files and folder
```
sed -i 's/yourprojectname/ENTER_YOUR_NAME_HERE/g' docker-compose.yml Dockerfile
```

### Step7
create your lumen directory project and clone developer lumen project.if you dont have any project you should create it with this link https://lumen.laravel.com/docs/6.x
```
mkdir lumen-app;cd lumen-app;git clone <Repo URL> <yourprojectname>
```

### Step8
Install composer
```
cd yourprojectname;docker run --rm -v $(pwd):/app composer install;cd ../..
```
if you have local repo run following command
```
cd yourprojectname;docker run --rm -v $(pwd):/app YOUR_PRIVATE_REGISTRY_URL/composer install;cd ../..
```

### Step9 (Optional)
Change your docker images with your local repository if you needed.


#### Notice
if you need create project you should follow this.otherwise skip this.
```
cd lumen-app;docker run --rm -v $(pwd):/app composer create-project --prefer-dist laravel/lumen yourprojectname
```

### Step11
change environment variable for lumen with following configuration
```
cp lumen-app/yourprojectname/.env.example lumen-app/yourprojectname/.env
```

Change Following Configuration on lumen-app/.env
```
DB_CONNECTION=mysql
DB_HOST=<DB HOST>
DB_PORT=<DB PORT>
DB_DATABASE=<DB NAME>
DB_USERNAME=<DB USERNAME>
DB_PASSWORD=<DB PASSWORD>
```

### Step12
change permission for lumen project to access on it
```
sudo chown -R $USER:$USER lumen-app/
```


### Step13
run docker compose command
```
docker-compose up -d --build
```


### Step14
Creating a User for MySQL
```
docker-compose exec db bash
```
and then 
```
mysql -u root -p
```

on mysql command line to see all databases run
```
show databases;
```

Grant Permission to your user on specific database
```
GRANT ALL ON laravel.* TO 'laraveluser'@'%' IDENTIFIED BY 'your_laravel_db_password';
```

Flush the privileges to notify the MySQL server of the changes:
```
FLUSH PRIVILEGES;
```
and exit from mysql and container mysql


### Step15
Check same step 14 for dblogging

### Step16
You can Migrate your laravel with following command
```
docker-compose exec app php artisan migrate
```

