# INFORMATION SYSTEM ADMINISTRATION AND DEVELOPMENT

## POSTGRESQL DEPLOYMENT ON UBUNTU SERVER

### USER GUIDE DOCUMENTATION
![1_rzJtXEetdwO96c9OMwH8Kw](https://github.com/user-attachments/assets/721b4e87-978d-464c-9ade-a8bce3e407ce)
---

Hello everyone,

Today, I will demonstrate how to install and configure the relational database management system (RDBMS) PostgreSQL, and create the necessary environment so that it can be used across your company or business environment. Please follow this documentation along with the video tutorial. Without the tutorial, you may face significant challenges during implementation.

---

### Installation and Configuration Steps

# Step 1: Install PostgreSQL
```bash
sudo apt-get install postgresql -y
```
# Step 2: Install Curl
```bash
sudo apt install curl
```
# Step 3: Add the pgAdmin Repository
```bash
curl -fsS https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/packages-pgadmin-org.gpg
```
# Step 4: Create the Repository Configuration File
```bash
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/packages-pgadmin-org.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'
```
# Step 5: Install pgAdmin for Both Desktop and Web Modes
```bash
sudo apt install pgadmin4
```
# Step 6: Install pgAdmin for Desktop Mode Only
```bash
sudo apt install pgadmin4-desktop
```
# Step 7: Install pgAdmin for Web Mode Only
```bash
sudo apt install pgadmin4-web
```
# Step 8: Configure the Web Server for pgAdmin
```bash
sudo /usr/pgadmin4/bin/setup-web.sh

```

# Step 9: Set Up pgAdmin Web Access
# You will be prompted to set a username and password for accessing pgAdmin.
# Enter the following credentials:
# - Email: demo@gmail.com
# - Password: demo2023

# Step 10: Configure PostgreSQL Database and User for Django
# (a) Create a PostgreSQL User
```bash
sudo -u postgres psql -c "CREATE USER management WITH PASSWORD 'management';"
```
# (b) Create a Database
```bash
sudo -u postgres psql -c "CREATE DATABASE inventory_mis;"
```
# (c) Grant Privileges
```bash
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE inventory_mis TO management;"
```
# (d) Grant Privileges on Schema
```bash
sudo -u postgres psql -d inventory_mis -c "GRANT ALL PRIVILEGES ON SCHEMA public TO management;"
```
# Step 11: Update Django Settings
# Open your settings.py file and configure the PostgreSQL database:
# settings.py
# Configure your Django application to connect to PostgreSQL:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'inventory_mis',
        'USER': 'management',
        'PASSWORD': 'management',
        'HOST': '10.0.2.15',
        'PORT': '5432',  # Default PostgreSQL port
    }
}
```
# Step 12: Open `pg_hba.conf` for Editing
```bash
sudo gedit /etc/postgresql/12/main/pg_hba.conf
```
# Add the following line:
```bash
host    all             all             10.0.2.15/32            md5
```
# Step 13: Open `postgresql.conf` for Editing
```bash
sudo nano /etc/postgresql/12/main/postgresql.conf
```
# Uncomment and modify the following line:
```bash
# listen_addresses = '*'
```

# Step 14: Allow Traffic on Port 5432
```bash
sudo ufw allow 5432
sudo ufw reload
```

# Step 15: Restart PostgreSQL
```bash
sudo systemctl restart postgresql
```

# Check the status of PostgreSQL
```bash
sudo systemctl status postgresql
```
# Enable the PostgreSQL service to start on boot
```bash
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

# Step 16: Verify Services
```bash
sudo systemctl is-active postgresql
sudo systemctl is-enabled postgresql
sudo systemctl status postgresql
sudo pg_isready
```

# If the services are not running, enable and start them with:
```bash
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

# Step 17: Access pgAdmin in Browser
# Open your browser and navigate to:
# - http://10.0.2.15/pgadmin4

# Step 18: Log in to pgAdmin
# After opening the page, log in with the following credentials:
# - Email: demo@gmail.com
# - Password: demo2023

# Step 19: Add a Server in pgAdmin
# Once logged in, add a new server with the following details:
# - Name: inventory_mis
# - Host: 10.0.2.15
# - Username: management
# - Password: management

# Step 20: Finalize Configuration in pgAdmin
# - After adding the server, ensure that it is connected properly and check the database settings.
# - Verify that the database `inventory_mis` is visible under the server `inventory_mis`.
# - You can now start creating and managing your databases through pgAdmin.
