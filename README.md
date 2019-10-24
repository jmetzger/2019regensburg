# MariaDB - Training - Stadt Regensburg 

## Gewusst wie: Apache / php / mysql
```
apt install apache2
apt install libapache2-mod-php
apt install php7.2-mysql
```
### Beispielcode index.php ###

```
<?php
$link = mysqli_connect("127.0.0.1", "sakila", "__streng_geheimes_passwort_das_nicht_Verratenwird", "sakila");

if (!$link) {
    echo "Fehler: konnte nicht mit MySQL verbinden." . PHP_EOL;
    echo "Debug-Fehlernummer: " . mysqli_connect_errno() . PHP_EOL;
    echo "Debug-Fehlermeldung: " . mysqli_connect_error() . PHP_EOL;
    exit;
}

echo "Erfolg: es wurde ordnungsgemäß mit MySQL verbunden! Die Datenbank \"datenbank\" ist toll." . PHP_EOL;
echo "Host-Informationen: " . mysqli_get_host_info($link) . PHP_EOL;

if ($result = mysqli_query($link, "select * from actor")) {
    printf("Select returned %d rows.\n", mysqli_num_rows($result));

    while ($row=mysqli_fetch_assoc($result)){

       print "<pre>";
       print_r ($row);
       print "</pre>";

    }
}

mysqli_close($link);
?>

```



## Gewusst wie shapes importieren:

### ogr2ogr cheatsheet ###
https://www.bostongis.com/PrinterFriendly.aspx?content_name=ogr_cheatsheet

### ogr2ogr installieren 
https://mothergeo-py.readthedocs.io/en/latest/development/how-to/gdal-ubuntu-pkg.html

#### Schritt 1: Installation auf Ubuntu 18.04. LTS ####
```
apt install -y python3.6-dev
add-apt-repository ppa:ubuntugis/ppa && sudo apt-get update
apt install -y gdal-bin

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
echo "create schema shapetest" | mysql -uroot -pirgendeinpassword 
ogr2ogr -f MySQL MySQL:shapetest,host=localhost,user=root,password=irgendeinpassword gis_osm_waterways_free_1.shp -nln shapefiles -update -overwrite -lco engine=INNODB
```

#### Schritt 4: Shapefile auslesen ####
```
ogrinfo MySQL:shapetest,user=root shapefiles -so
```
