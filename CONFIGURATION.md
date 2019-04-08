

## Configuration

This guide covers the installation of all required services as well as the configuration of AWX itself. You can skip the next section `Install Requirements` if you have already installed all required services.

### Install Requirements

install all required services

```bash 
sudo apt install memcached rabbitmq-server postgresql postgresql-contrib
```


put all services in autostart and ensure they are running

```bash
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server

sudo systemctl enable postgresql
sudo systemctl start postgresql

sudo systemctl enable memcached
sudo systemctl start memcached
```

### Run initial Setup

create a postgres user and database

```bash
sudo -u postgres createuser -S awx
sudo -u postgres createdb -O awx awx
```

All AWX related credentials are stored in a separate file. Copy or move our example file to the target destination `/etc/awx/credentials.py`.  

```bash
sudo cp /etc/awx/credentials.py.example /etc/awx/credentials.py
```
modify the credentials if necessary.

NOTE: if your PostgreSQL database is not running on the same host or you want to use a password based login, you have to uncomment the USER, PASSWORD, HOST and PORT option in `/etc/awx/settings.py`!  


execute the awx migration scripts

```bash
sudo -u awx AWX_SETTINGS_FILE=/etc/awx/settings.py awx-manage migrate
```
   
now create your first AWX user and start the instance provisioning

```bash
sudo -u awx bash
export AWX_SETTINGS_FILE=/etc/awx/settings.py
echo "from django.contrib.auth.models import User; User.objects.create_superuser('admin', 'root@localhost', 'password')" | awx-manage shell
awx-manage provision_instance --hostname=$(hostname)
awx-manage register_queue --queuename=tower --hostnames=$(hostname)
awx-manage create_preload_data
```


If you want to run the migrations automatically create a file called `.enable_auto_migrations` in `/etc/awx/`.

```bash 
touch /etc/awx/.enable_auto_migrations
```
