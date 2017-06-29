---
layout: default
---
<style>
#plot {
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
</style>

<div id="home">
  <div id="plot"></div>
  <div id="content">
    <h2>Plots</h2>
    <p><b>Product:</b> <select id="product-selector">
                         <option value="rain">Rainfall</option>
                       </select></p>
<!--    <p><b>Period:</b> <input type="radio" name="period-selector" value="mission">Mission
                      <input type="radio" name="period-selector" value="year">Annual
                      <input type="radio" name="period-selector" value="month">Month</p> -->
    <div name="time-div" styl="display: none; visibility: hidden;"><p><b>Time:</b> <select id="time-selector" onchange="changetime();" onclick="changetime();">
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

<script>
img = document.createElement('img');
imgDiv = document.getElementById('plot');
oldTime = document.getElementById('time-selector').value;
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
