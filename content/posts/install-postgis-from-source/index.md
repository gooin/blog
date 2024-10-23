---
title: "源码编译安装PostGIS"
date: 2019-11-20
draft: false
description: ""
tags: ["GIS", "tools"]
---

## Step1 安装基本的依赖环境

### 安装 `geos`

```bash
wget http://download.osgeo.org/geos/geos-3.8.1.tar.bz2
tar -xf geos-3.8.1.tar.bz2
cd geos-3.8.1/
./configure
make
make install
```

### 安装 `proj`

```bash
wget https://download.osgeo.org/proj/proj-6.3.1.tar.gz
tar -xf proj-6.3.1.tar.gz
cd proj-6.3.1/
// 缺少sqlite3,先安装
apt-get install libsqlite3-dev
apt install sqlite3
./configure
make

```

### 安装`GDAL`

```bash
wget https://github.com/OSGeo/gdal/releases/download/v3.0.4/gdal-3.0.4.tar.gz
tar -xf gdal-3.0.4.tar.gz
cd gdal-3.0.4/
./configure

```

### 安装`LibXML2`

```bash
sudo apt-get install libxml2
sudo apt-get install libxml2-dev

```

### 安装`json-c`

```bash
apt install libjson-c-dev

```

## Step2 下载 `PostGIS` 源码进行编程

```bash
wget https://download.osgeo.org/postgis/source/postgis-3.0.1.tar.gz
tar xvzf postgis-3.0.1.tar.gz
cd postgis-3.0.1
./configure --with-pgconfig=/export/home/postgres/11.5/bin/pg_config

apt-get install byacc
apt install protobuf-compiler libprotobuf-c-dev libprotobuf-c1

make //出问题就 make clean
make install
```

### 文档

`http://postgis.net/docs/manual-dev/`

### 创建模板数据库

```bash
create database postgis_template;
\c postgis_template;
CREATE EXTENSION IF NOT EXISTS plpgsql;
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_raster; -- OPTIONAL
CREATE EXTENSION postgis_topology; -- OPTIONAL
```

### 从模板数据库创建数据库

```bash
CREATE DATABASE my_spatial_db TEMPLATE=postgis_template;
\c my_spatial_db;
```

### POINT

Creating a table with 2D point geography when srid is not specified defaults to 4326 WGS 84 long lat

`CREATE TABLE ptgeogwgs(gid serial PRIMARY KEY, geog geography(POINT));`

Creating a table with z coordinate point and explicitly specifying srid

`CREATE TABLE ptzgeogwgs84(gid serial PRIMARY KEY, geog geography(POINTZ,4326) );`

### LINESTRING

`CREATE TABLE lgeog(gid serial PRIMARY KEY, geog geography(LINESTRING) );`

### POLYGON

### polygon NAD 1927 long lat

`CREATE TABLE lgeognad27(gid serial PRIMARY KEY, geog geography(POLYGON,4267) );`

```sql
CREATE TABLE global_points (
    id SERIAL PRIMARY KEY,
    name VARCHAR(64),
    location GEOGRAPHY(POINT,4326)
);
```

所有创建的表都在 `geography_columns` 系统视图中

`SELECT * FROM geography_columns;`

```bash
 f_table_catalog | f_table_schema | f_table_name  | f_geography_column | coord_dimension | srid |    type
-----------------+----------------+---------------+--------------------+-----------------+------+------------
 my_spatial_db   | public         | ptgeogwgs     | geog               |               2 | 4326 | Point
 my_spatial_db   | public         | ptzgeogwgs84  | geog               |               3 | 4326 | PointZ
 my_spatial_db   | public         | lgeog         | geog               |               2 | 4326 | LineString
 my_spatial_db   | public         | global_points | location           |               2 | 4326 | Point
(4 rows)

```

You can insert data into the table the same as you would if it was using a GEOMETRY column:

```sql
-- Add some data into the test table
INSERT INTO global_points (name, location) VALUES ('Town', 'SRID=4326;POINT(-110 30)');
INSERT INTO global_points (name, location) VALUES ('Forest', 'SRID=4326;POINT(-109 29)');
INSERT INTO global_points (name, location) VALUES ('London', 'SRID=4326;POINT(0 49)');
```

Creating an index works the same as GEOMETRY. PostGIS will note that the column type is GEOGRAPHY and create an appropriate sphere-based index instead of the usual planar index used for GEOMETRY.

```sql
-- Index the test table with a spherical index
  CREATE INDEX global_points_gix ON global_points USING GIST ( location );
```

Query and measurement functions use units of meters. So distance parameters should be expressed in meters, and return values should be expected in meters (or square meters for areas).

```sql
-- Show a distance query and note, London is outside the 1000km tolerance
SELECT name FROM global_points WHERE ST_DWithin(location, 'SRID=4326;POINT(-110 29)'::geography, 1000000);
```

### 导出空间数据导出为 shp

`cd /export/home/postgres/postgis-3.1.0dev/loader`

./pgsql2shp -f "/home/gisdata/pg_export/xxx" -h localhost -u postgres -P postgres my_spatial_db "select \* from global_points"

### shp 导入为空间数据

`cd /export/home/postgres/postgis-3.1.0dev/loader`

```bash
./shp2pgsql -c -d -D -s 4326 -W utf-8 -i -I /home/gisdata/3d/hangzhou/hangzhou_shp.shp > hz.sql
psql -d my_spatial_db -f hz.sql

//或者用管道符号
./shp2pgsql -c -d -D -s 4326 -W utf-8 -i -I /home/gisdata/3d/hangzhou/hangzhou_shp.shp | psql -d my_spatial_db -f hz.sql
```

Changeme_1234567
