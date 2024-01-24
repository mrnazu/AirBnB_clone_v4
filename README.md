## AirBnB Clone v4 - Technical Overview

### Project Overview
The AirBnB Clone project aims to replicate the functionality of the AirBnB application and website. This includes the implementation of a comprehensive ecosystem, covering the database, storage, RESTful API, web framework, and front end. The application supports two storage engine models:

![](https://github.com/mrnazu/AirBnB_clone_v4/assets/108541991/b342b09b-31fe-46c4-8bd7-e41b3b500e77)

1. **File Storage Engine:**
   - Implementation: `/models/engine/file_storage.py`

2. **Database Storage Engine:**
   - Implementation: `/models/engine/db_storage.py`
   - Setup scripts: `setup_mysql_test.sql` and `setup_mysql_test.sql` (for more details, see below)

### Environment
- **Operating System:** Ubuntu 14.04 LTS
- **Language:** Python 3.4.3
- **Web Server:** Nginx/1.4.6
- **Application Server:** Flask 0.12.2, Jinja2 2.9.6
- **Web Server Gateway:** Gunicorn (version 19.7.1)
- **Database:** MySQL Ver 14.14 Distrib 5.7.18
- **Documentation:** Swagger (flasgger==0.6.6)
- **Style:**
  - Python: PEP 8 (v. 1.7.0)
  - Web Static: W3C Validator
  - Bash: ShellCheck 0.3.3

### Configuration Files
- **Directory:** `/config/`
- **Nginx Configuration:**
  - Path: `/etc/nginx/sites-available/default`
  - Enabled site: Symbolic link to the configuration file
- **Upstart Script:**
  - Path: `/etc/init/[FILE_NAME.conf]`
  - Service Start: `$ sudo start airbnb.conf`
  - Gunicorn Command: `$ gunicorn --bind 127.0.0.1:8001 wsgi.wsgi:web_flask.app`

### Web Server Gateway Interface (WSGI)
- All integration with Gunicorn is managed through Upstart `.conf` files.
- Python code for WSGI is in the `/wsgi/` directory, running designated Flask Applications.

### Setup
The project includes various setup scripts for automation, maintenance, or scaling:
- `dev/setup.sql`: Drops test and dev databases, then reinitializes the database.
  - Usage: `$ cat dev/setup.sql | mysql -uroot -p`
- `setup_mysql_dev.sql`: Initializes the dev database with MySQL for testing.
  - Usage: `$ cat setup_mysql_dev.sql | mysql -uroot -p`
- `setup_mysql_test.sql`: Initializes the test database with MySQL for testing.
  - Usage: `$ cat setup_mysql_test.sql | mysql -uroot -p`
- `0-setup_web_static.sh`: Sets up Nginx web server config file & the file structure.
  - Usage: `$ sudo ./0-setup_web_static.sh`
- `3-deploy_web_static.py`: Utilizes functions from `1-pack_web_static.py` & `2-do_deploy_web_static.py` using Fabric3 for creating a `.tgz` file of all local web static files and deploying them.
  - Usage: `$ fab -f 3-deploy_web_static.py deploy -i ~/.ssh/holberton -u ubuntu`

### Testing
- **Unittest:**
  - File Storage Engine Model: `$ python3 -m unittest discover -v ./tests/`
  - Database Storage Engine Model:
    ```
    $ HBNB_MYSQL_USER=hbnb_test HBNB_MYSQL_PWD=hbnb_test_pwd \
    HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_test_db HBNB_TYPE_STORAGE=db \
    python3 -m unittest discover -v ./tests/
    ```
- **All Tests:**
  - Bash script `init_test.sh` executes all tests for both File Storage & Database Engine Models.
    - Checks PEP8 style
    - Runs all unittests
    - Runs all W3C_validator tests
    - Cleans up `__pycache__` directories and the storage file, `file.json`
    - Usage: `$ ./dev/init_test.sh`

### CLI Interactive Tests
- Utilizes the `cmd` library to run tests in an interactive command-line interface.
  - File Storage Engine Model: `$ ./console.py`
  - Database Storage Engine Model:
    ```
    $ HBNB_MYSQL_USER=hbnb_test HBNB_MYSQL_PWD=hbnb_test_pwd \
    HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_test_db HBNB_TYPE_STORAGE=db \
    ./console.py
    ```
  - Detailed description of all tests: `(hbnb) help help`
