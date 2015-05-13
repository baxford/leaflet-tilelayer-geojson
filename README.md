# Leaflet GeoJSON Clustered Tile Layer
Renders GeoJSON tiles on an L.GeoJSON layer and allows clustering of results.
This is probably mostly useful for single point data in the GeoJSON results

(forked from glenrobertson/leaflet-tilelayer-geojson)

## Docs

### Usage example
The following example shows how to render a GeoJSON Tile Layer for US state GeoJSON tiles. [See demo](http://bl.ocks.org/glenrobertson/6203331).

        var style = {
            "clickable": true,
            "color": "#00D",
            "fillColor": "#00D",
            "weight": 1.0,
            "opacity": 0.3,
            "fillOpacity": 0.2
        };
        var hoverStyle = {
            "fillOpacity": 0.5
        };

        var geojsonURL = 'http://polymaps.appspot.com/state/{z}/{x}/{y}.json';
        var maxZoom = 18;
        var geojsonClusterTileLayer = new L.TileLayer.ClusteredGeoJSON(geojsonURL, {
                maxZoom: maxZoom,
                clipTiles: false,
                maxClusterRadius: 80, //A cluster will cover at most this many pixels from its center
                showCoverageOnHover: false,
                zoomToBoundsOnClick: true,
                singleMarkerMode: false,
                disableClusteringAtZoom: maxZoom,
                iconCreateFunction: function (cluster) {
                    var html = '<div class="cluster-icon"><b>' + cluster.getChildCount() + '</b></div>';
                    return new L.DivIcon({
                        html: html
                    });
                },
                unique: function (feature) {
                    return feature.properties.id;
                },
                pointToLayer: function (feature, latlng) {
                    var html = '<div class="my-icon"></div>';
                    return L.marker(latlng,
                        {
                            icon: L.divIcon({
                                html: html
                            })
                        }
                    );
                },
                style: function (feature) {
                    return {color: "#ff0000"};
                },
                onEachFeature: function (feature, layer) {
                    if (feature.properties) {
                        var popupString = '<div class="place-popup">';
                        popupString += '<h4>' + feature.properties.name + '</h4>';
                        popupString += '</div>';
                        layer.bindPopup(popupString);
                    }
                    if (!(layer instanceof L.Point)) {
                        layer.on('mouseover', function () {
                        });
                        layer.on('mouseout', function () {
                        });
                    }
                }
            }
        );
        map.addLayer(geojsonClusterTileLayer);

### Constructor
    L.TileLayer.ClusteredGeoJSON( <String> urlTemplate, <GeoJSONTileLayer options> options?, <GeoJSON options> geojsonOptions? )

### URL Template
A string of the following form, that returns valid GeoJSON.

    'http://{s}.somedomain.com/blabla/{z}/{x}/{y}.json'

### ClusteredGeoJSON options
* `clipTiles (boolean) (default = false)`: If `true`, clips tile feature geometries to their tile boundaries using SVG clipping.
* `unique (function)`: If set, the feature's are grouped into GeometryCollection GeoJSON objects. Each group is defined by the key returned by this function, with the feature object as the first argument.
* additional options from L.markerClusterGroup

### GeoJSON options
Options that will be passed to the resulting L.GeoJSON layer: [http://leafletjs.com/reference.html#geojson-options](http://leafletjs.com/reference.html#geojson-options)


## Contributors
Thanks to the following people who contributed:

* [Nelson Minar](https://github.com/NelsonMinar)
* [Alex Barth](https://github.com/lxbarth)
* [Pawel Paprota](https://github.com/ppawel)
* [Mick Thompson](https://github.com/dthompson)
