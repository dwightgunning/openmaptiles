
## OMT Start-to-finish steps

(From the README.md)

```
make clean # clean / remove existing build files
make # generate build files
make start-db # start up the database container.
make import-data # Import external data from OpenStreetMapData, Natural Earth and OpenStreetMap Lake Labels.
make download area=albania # download albania .osm.pbf file -- can be skipped if a .osm.pbf file already existing
make import-osm # import data into postgres
make import-wikidata # import Wikidata
make import-sql # create / import sql functions
make generate-bbox-file # compute data bbox -- not needed for the whole planet
make generate-tiles-pg # generate tiles
```

## Step-by-step guide

### Download OSM Areas from Geofabriek

https://ckochis.com/vector-tiles-from-osm

Geofabriek areas to download:

{
'andorra': true,
'australia': true,
'finland': true,
'germany': ['duesseldorf-regbez', 'berlin'],
'spain': true,
'tanzania': true,
'netherlands': true,
'canada': ['alberta', 'british-columbia'],
'france': ['languedoc-roussillon', 'midi-pyrenees'],
'great-britain': ['cornwall', 'devon'],
'ireland-and-northern-ireland': true,
'portugal': true,
'slovenia': true,
'spain': ['cataluna'],
'united-states': ['arizona', 'california', 'colorado', 'nebraska', 'nevada', 'oregon', 'washington', 'utah']
}

_TODO_: Note the OpenMapTiles download commands ...

```
make download-geofabrik {area}
```

### 2. Merge the OSM Area files (OSM.PDF)

The Osmium docker image exposes the `merge` tool.

`merge [input-file-1.osm.pbf, ... ] -o outputfile.osm.pbf`

My-World:

```
docker run -v /Users/dwight/Documents/projects/git/openmaptiles/data:/data -it osmium-tool:latest merge data/alberta.osm.pbf data/andorra.osm.pbf data/arizona.osm.pbf data/australia.osm.pbf data/british-columbia.osm.pbf data/california.osm.pbf data/cataluna.osm.pbf data/colorado.osm.pbf data/cornwall.osm.pbf data/devon.osm.pbf data/duesseldorf-regbez.osm.pbf data/finland.osm.pbf data/france.osm.pbf data/ireland-and-northern-ireland.osm.pbf data/languedoc-roussillon.osm.pbf data/midi-pyrenees.osm.pbf data/nebraska.osm.pbf data/netherlands.osm.pbf data/nevada.osm.pbf data/oregon.osm.pbf data/portugal.osm.pbf data/spain.osm.pbf data/tanzania.osm.pbf data/washington.osm.pbf data/utah.osm.pbf -o /data/out/my-world.osm.pbf
```

Corners:

```
docker run -v /Users/dwight/Documents/projects/git/openmaptiles/osm-data:/data -it osmium-tool:latest merge data/andorra.osm.pbf data/australia.osm.pbf data/british-columbia.osm.pbf data/cornwall.osm.pbf data/devon.osm.pbf data/finland.osm.pbf data/ireland-and-northern-ireland.osm.pbf data/tanzania.osm.pbf data/us-westcoast.osm.pbf -o /data/out/mycorners.osm.pbf
```

europetest:

```
docker run -v /Users/dwight/Documents/projects/git/openmaptiles/osm-data:/data -it osmium-tool:latest merge data/andorra.osm.pbf data/devon.osm.pbf data/finland.osm.pbf data/ireland-and-northern-ireland.osm.pbf -o /data/out/europetest.osm.pbf
```


### 3.

1. Planet: Z0-8
2. Regions: Z9-13
3. Cities & Tracks: Z14-20
