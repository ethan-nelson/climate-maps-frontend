---
layout: default
---
<link rel="stylesheet" href="{{ 'static/leaflet-1.0.3/leaflet.css' | prepend: site.baseurl }}" />
<script src="{{ 'static/leaflet-1.0.3/leaflet.js' | prepend: site.baseurl }}"></script>
<!-- <script src="{{ 'static/leaflet.vectorgrid-1.2.0/Leaflet.VectorGrid.bundled.js' | prepend: site.baseurl }}"></script> -->
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
    <p><b>Period:</b> <input type="radio" name="period-selector" id="period-selector-mission" value="mission" onchange="changeperiod(); changetime();" checked="checked">Mission
                      <input type="radio" name="period-selector" id="period-selector-year" value="year" onchange="changeperiod(); changetime();">Annual
                      <input type="radio" name="period-selector" id="period-selector-month" value="month" onchange="changeperiod(); changetime();">Month</p>
    <div name="time-div" styl="display: none; visibility: hidden;"><p><b>Time:</b> <span id="time-options"></span></p></div>
    <p><a href="" id="download-link">Download data</a></p>

  </div>

<!-- <script src="{{ 'test.js' | prepend: site.baseurl }}"></script> -->

<script>
function getColor(d) {
    return d > 10 ? '#A63603' :
           d > 8 ? '#E6550D' :
           d > 6 ? '#FD8D3C' :
           d > 4 ? '#FDAE6B' :
           d > 2 ? '#FDD0A2' :
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

function changeperiod() {
    div = '<select id="time-selector" onchange="changetime();" onclick="changetime();">';
    if (document.getElementById('period-selector-month').checked) {
        for (i = 2007; i < 2011; i++) {
            for (j = 1; j < 13; j++) {
                div += '<option value="' + i.toString() + ('0'+j.toString()).slice(-2) + '">' + i.toString() + '-' + ('0'+j.toString()).slice(-2) + '</option>';
            }
        }
    } else if (document.getElementById('period-selector-year').checked) {
        for (i = 2007; i < 2011; i++) {
            div += '<option value="' + i.toString() + '">' + i.toString() + '</option>';
        }
    } else if (document.getElementById('period-selector-mission').checked) {
        div += '<option value="">Mission</option>';
    }
    div += '</select>';
    document.getElementById('time-options').innerHTML = div;
}

changeperiod();


var oldTime = document.getElementById('time-selector').value;
var geojson = L.geoJson.ajax('{{ 'data/gpcp-' | prepend: site.baseurl }}'+oldTime+'.geojson', {onEachFeature: onEachFeature, style: style});
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
