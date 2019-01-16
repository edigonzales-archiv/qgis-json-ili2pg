# qgis-json-ili2pg


## ili2pg
Devel-Version von ili2pg builden:
```
git clone https://github.com/claeis/ili2db.git ili2db_master
cd ili2db_master
gradle build ili2pgBindist -x usrdoc
```
Dist-Zip unzippen.

JSON-Mapping-Metattribut `!!@ili2db.mapping=JSON` in INTERLIS-Modell manuell setzen (funktionieren Meta-Attribute im UML-Editor?).

Pub-Schema in DB erstellen:
```
java -jar /Users/stefan/sources/ili2db_master/dist/ili2pg-4.0.0-SNAPSHOT-bindist/ili2pg-4.0.0-SNAPSHOT.jar --dbhost 192.168.50.8 --dbdatabase edit --dbusr ddluser --dbpwd ddluser --nameByTopic --strokeArcs --disableValidation --coalesceJson --modeldir "http://models.geo.admin.ch;." --models SO_Nutzungsplanung_Publikation_20190116 --dbschema arp_npl_pub --schemaimport
```

Gretl-Job für Datenumbau ausführen:

```
cd gretl
gradle createSchemaNpl importNplDataset transferArpNpl
```

Daten im Pubmodell exportieren:
```
java -jar /Users/stefan/sources/ili2db_master/dist/ili2pg-4.0.0-SNAPSHOT-bindist/ili2pg-4.0.0-SNAPSHOT.jar --dbhost 192.168.50.8 --dbdatabase edit --dbusr ddluser --dbpwd ddluser --nameByTopic --strokeArcs --disableValidation --coalesceJson --modeldir "http://models.geo.admin.ch;." --models SO_Nutzungsplanung_Publikation_20190116 --dbschema arp_npl_pub --export fubar_json.xtf
```

### Test mit ili2db-Test-Daten
```
java -jar /Users/stefan/sources/ili2db_master/dist/ili2pg-4.0.0-SNAPSHOT-bindist/ili2pg-4.0.0-SNAPSHOT.jar --dbhost 192.168.50.8 --dbdatabase edit --dbusr ddluser --dbpwd ddluser --nameByTopic --strokeArcs --disableValidation --coalesceJson --modeldir "." --models Json23 --dbschema json_test --schemaimport


java -jar /Users/stefan/sources/ili2db_master/dist/ili2pg-4.0.0-SNAPSHOT-bindist/ili2pg-4.0.0-SNAPSHOT.jar --dbhost 192.168.50.8 --dbdatabase edit --dbusr ddluser --dbpwd ddluser --nameByTopic --strokeArcs --disableValidation --coalesceJson --modeldir "." --models Json23 --dbschema json_test --import Json23a.xtf

java -jar /Users/stefan/sources/ili2db_master/dist/ili2pg-4.0.0-SNAPSHOT-bindist/ili2pg-4.0.0-SNAPSHOT.jar --dbhost 192.168.50.8 --dbdatabase edit --dbusr ddluser --dbpwd ddluser --nameByTopic --strokeArcs --disableValidation --coalesceJson --modeldir "." --models Json23 --dbschema json_test --export Json23a_export.xtf

```
```
"[{\"r\":10,\"@type\":\"Json23.TestA.Farbe\",\"g\":11,\"b\":12,\"name\":\"f1\",\"active\":false},{\"@type\":\"Json23.TestA.Farbe\",\"r\":20,\"g\":21,\"b\":22,\"name\":\"f2\",\"active\":false}]"

"[{\"@type\":\"Json23.TestA.Farbe\",\"r\":10,\"g\":11,\"b\":12,\"name\":\"f1\",\"active\":false},{\"@type\":\"Json23.TestA.Farbe\",\"r\":20,\"g\":21,\"b\":22,\"name\":\"f2\",\"active\":false}]"
"[{\"@type\":\"SO_Nutzungsplanung_Publikation_20190116.Nutzungsplanung.Farbe\",\"r\":10,\"g\":11,\"b\":12,\"name\":\"f1\",\"active\":false},{\"@type\":\"SO_Nutzungsplanung_Publikation_20190116.Nutzungsplanung.Farbe\",\"r\":20,\"g\":21,\"b\":22,\"name\":\"f2\",\"active\":false}]"
```

## JSON in QGIS (QML-Widget)

```
import QtQuick 2.0
import QtQuick.Controls 1.4

TableView {
    width: 600
    model: expression.evaluate("\"dokumente\"")
    TableViewColumn {
        role: "titel"
        title: "Titel"
        width: 200
    }
}
```

Testen, ob JSON von QGIS lesbar:

```
for feature in iface.activeLayer().getFeatures():
    print(repr(feature['dokumente']))
```

