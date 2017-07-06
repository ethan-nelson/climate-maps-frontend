---
layout: default
title: Plots
---
<style>
#plot {
  position: absolute;
  top: 56px;
  bottom: 0;
  right: 260px;
  left: 0;
}
#content {
  position: absolute;
  top: 56px;
  bottom: 0;
  right: 5px;
  width: 250px;
}
</style>

<div id="home">
  <div id="plot"></div>
  <div id="content">
    <h2>Plots</h2>
    <p><b>Product:</b> <select id="product-selector">
                         <option value="rain">Rainfall</option>
                       </select></p>
    <p><b>Period:</b><br />
    <label><input type="radio" name="period-selector" id="period-selector-mission" value="mission" onchange="changeperiod(); changetime();" checked="checked"><span class="checkable">Mission</span></label><br />
                      <label><input type="radio" name="period-selector" id="period-selector-year" value="year" onchange="changeperiod(); changetime();"><span class="checkable">Annual</span></label><br />
                      <label><input type="radio" name="period-selector" id="period-selector-month" value="month" onchange="changeperiod(); changetime();"><span class="checkable">Month</span></label></p>
    <div name="time-div" styl="display: none; visibility: hidden;"><p><b>Time:</b> <span id="time-options"></span></p></div>
    <p><a href="{{ 'download' | prepend: site.baseurl }}" id="download-link">Download data</a></p>

<script>
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

var img = document.createElement('img');
var imgDiv = document.getElementById('plot');
var oldTime = document.getElementById('time-selector').value;
img.src = '{{ 'data/gpcp-' | prepend: site.baseurl }}'+oldTime+'.png';
imgDiv.appendChild(img);


function changetime() {
    var newTime = document.getElementById('time-selector').value;
    if (newTime != oldTime) {
        img.src = '{{ 'data/gpcp-' | prepend: site.baseurl }}'+newTime+'.png';
        oldTime = newTime;
    }
}
</script>

  </div>
</div>
