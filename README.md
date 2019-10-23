# MariaDB - Training - Stadt Regensburg 

## Gewusst wie :  
### ocr2ocr installieren 
https://mothergeo-py.readthedocs.io/en/latest/development/how-to/gdal-ubuntu-pkg.html

#### Schritt 1: Installation auf Ubuntu 18.04. LTS ####
```
apt install python3.6-dev
add-apt-repository ppa:ubuntugis/ppa && sudo apt-get update
apt install gdal-bin

ogrinfo --version
```

#### Schritt 2: Shapefile (Beispiel) runterladen ####

```
# von http://download.geofabrik.de/
cd /usr/src
wget http://download.geofabrik.de/antarctica-latest-free.shp.zip
mkdir antarctica 
mv antarctica-latest-free.shp.zip antarctica 
apt install unzip 
cd antarctica
unzip antarctica-latest-free.shp.zip
```

#### Schritt 3: Datenbank anlegen und Shapes importieren ####

```
echo "shapetest" | mysql -uroot -pirgendeinpassword 
ogr2ogr -f MySQL MySQL:shapetest,host=localhost,user=root,password=irgendeinpassword gis_osm_transport_free_1.shp -nln shapefiles -update -overwrite -lco engine=INNODB
```
