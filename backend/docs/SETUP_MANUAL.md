# Manual Setup


### 1. Pre-install
- Python 3.7
- PostgreSQL > 9.0
- PostGIS > 2.0

### 2. DB settings
用 superuser 進 PostgreSQL
```
sudo su postgres
psql
```

建立一個可以開 DB 的 user 給 server
```
CREATE USER "disfactory" CREATEDB;
\password disfactory
```

開一個 DB
```
CREATE DATABASE disfactory_data OWNER "disfactory";
```

打開 PostGIS extention，但要先[安裝](https://postgis.net/install/)在系統上。
```
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_topology;
```

設定好之後，記得也要改環境變數，或是直接改動 `.env` 裡面的值。

(optional) 如果要跑單元測試，需要一個可以 create extension 的 user。很可惜的是目前 PostgreSQL 只有 superuser 可以這樣做。
```
ALTER ROLE "disfactory" SUPERUSER;
```

### 3. Environment variables
使用環境變數來設定專案，用 `python-dotenv` 來讀取。請參考 `.env.sample` 並複製一份 `.env`。

### 4. Install python packages
```
apt-get install binutils libproj-dev gdal-bin
pipenv install --dev --python=$(which python3)`
pipenv shell
```

### 5. DB migration
```
python manage.py migrate
```
NOTE: 別設 `DISFACTORY_BACKEND_LOG_LEVEL` 為 `DEBUG` ，因為 logger 裡使用到了 `django-db-logger`。Migration 結束之後再設成你想要的 level。
