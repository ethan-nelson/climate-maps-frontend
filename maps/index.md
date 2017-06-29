---
layout: default
---
<link rel="stylesheet" href="{{ 'static/leaflet-1.0.3/leaflet.css' | prepend: site.baseurl }}" />
<script src="{{ 'static/leaflet-1.0.3/leaflet.js' | prepend: site.baseurl }}"></script>
<script src="{{ 'static/leaflet.vectorgrid-1.2.0/Leaflet.VectorGrid.bundled.js' | prepend: site.baseurl }}"></script>
<script src="{{ 'static/leaflet-ajax/leaflet.ajax.min.js' | prepend: site.baseurl }}"></script>
<style>
#map {
  position: absolute;
  top: 56px;
  bottom: 0;
  right: 300px;
  left: 0;
}
#content {
  position: absolute;
  top: 56px;
  bottom: 0;
  right: 0;
  width: 295px;
}
.info {
  padding: 4px 4px;
  font: 14px/16px Arial;
  background: white;
  background: rgba(255,255,255,0.7);
  box-shadow: 0 0 15px rgba(0,0,0,0.2);
  border-radius: 5px;
}
</style>

<div class="home">
  <div id="map"></div>
  <div id="content">
    <h2>Interactive Maps</h2>
    <p><b>Product:</b> <select id="product-selector">
                         <option value="rain">Rainfall</option>
                       </select></p>
<!--    <p><b>Period:</b> <input type="radio" name="period-selector" value="mission">Mission
                      <input type="radio" name="period-selector" value="year">Annual
                      <input type="radio" name="period-selector" value="month">Month</p> -->
    <div name="time-div" styl="display: none; visibility: hidden;"><p><b>Time:</b> <select id="time-selector" onchange="changetime();">
                        <option value="00">2007-01</option>
                        <option value="01">2007-02</option>
                        <option value="02">2007-03</option>
                        <option value="03">2007-04</option>
                        <option value="04">2007-05</option>
                        <option value="05">2007-06</option>
                        <option value="06">2007-07</option>
                        <option value="07">2007-08</option>
                        <option value="08">2007-09</option>
                        <option value="09">2007-10</option>
                        <option value="10">2007-11</option>
                        <option value="11">2007-12</option>
                    </select></p></div>




  </div>

<!-- <script src="{{ 'test.js' | prepend: site.baseurl }}"></script> -->

<script>
function getColor(d) {
    return d > 5 ? '#A63603' :
           d > 4 ? '#E6550D' :
           d > 3 ? '#FD8D3C' :
           d > 2 ? '#FDAE6B' :
           d > 1 ? '#FDD0A2' :
           d > 0 ? '#FEEDDE' :
                   '#FFFFFF';
};

function style(feature) {
    return {
        fillColor: getColor(feature.properties.data),
        fillOpacity: 0.65,
        stroke: false
    };
};

var map = L.map('map', {worldCopyJump: true}).setView([35, 0], 3);
L.tileLayer('http://lecuyer.aos.wisc.edu/maps/{z}/{x}/{y}.png').addTo(map);

var info = L.control();

info.onAdd = function(map) {
    this._div = L.DomUtil.create('div', 'info');
    this.update();
    return this._div;
};

info.update = function(props) {
    this._div.innerHTML = '<h3>Rainfall</h3><p>' + (props ? props.data+' mm/day' : 'Hover over a point') + '</p>';
};

info.addTo(map);

function highlightFeature(e) {
    layer = e.target;
    info.update(layer.feature.properties);
};

function onEachFeature(feature, layer) {
    layer.on({
        mouseover: highlightFeature
    });
}

//geojson = L.geoJson(test, {onEachFeature: onEachFeature, style: style}).addTo(map);

oldTime = document.getElementById('time-selector').value;
var geojson = L.geoJson.ajax('{{ 'data/gpcp-' | prepend: site.baseurl }}'+oldTime+'.geojson');
geojson.addTo(map)

function changetime() {
    var newTime = document.getElementById('time-selector').value;
    if (newTime != oldTime) {
        geojson.refresh('{{ 'data/gpcp-' | prepend: site.baseurl }}'+newTime+'.geojson');
        oldTime = newTime;
    }
}

/*
var vectorGrid = L.vectorGrid.slicer( test, {
    rendererFactory: L.svg.tile, interactive: true,
       vectorTileLayerStyles: {
           sliced: function(properties, zoom) {
               var p = Math.round(properties.data / 2) + 1;
               return {
                   fillColor: p == 0 ? '#FEEDDE' :
                              p == 1 ? '#FDD0A2' :
                              p == 2 ? '#FDAE6B' :
                              p == 3 ? '#FD8D3C' :
                              p == 4 ? '#E6550D' : '#A63603',
                   fillOpacity: 0.65,
                   stroke: false,
                   fill: true
               }
           }
       }
       })
       .on('mouseover', function(e) {
           var properties = e.layer.properties;
           L.popup().setContent(properties.data+' mm/day')
               .setLatLng(e.latlng)
               .openOn(map);
       })
            .addTo(map);
*/

</script>

</div>
