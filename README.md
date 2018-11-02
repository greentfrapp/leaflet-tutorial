# leaflet-tutorial

This is the accompanying repository to my Leaflet tutorial [here](https://greentfrapp.github.io/2018/10/06/leaflet-tutorial.html).

Just download this repository as a zip file (click the bright green "Clone or download" button above and then click "Download ZIP").

The HTML files can be opened as standalone files, just double-click to open with your default browser. The files also build on top of one another, in increasing complexity (ie. `2-multiple-layers.html` builds on top of `1-basic.html`). The last HTML should display something like the interactive map below.

Refer to the [tutorial](https://greentfrapp.github.io/2018/10/06/leaflet-tutorial.html) for more details!

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.4/dist/leaflet.css" integrity="sha512-puBpdR0798OZvTTbP4A8Ix/l+A4dHDD0DGqYW6RQ+9jxkRFclaxxQb/SJAWZfWAkuyeQUytO7+7N4QKrDh+drA==" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.3.4/dist/leaflet.js" integrity="sha512-nMMmRyTVoLYqjP9hrbed9S+FzjZHW5gY1TWCHA5ckwXZBadntCNs8kEqAWdrb9O7rxbCaA4lKTIWjDXZxflOcA==" crossorigin="">
</script>
<style>
	/* Here we decide the size of the map */
	div.map {
		height: 500px;
		width: 500px;
		margin-left: auto;
		margin-right: auto;
	}
	div.infobox {
		padding: 8px;
		box-shadow: 0 0 15px rgba(0,0,0,0.2);
		border-radius: 8px;
		font-size: 18px;
	}
</style>

<div id='geojson-infobox' class='map'>
</div>

<script>
// Code for #infobox
var apiKey = 'pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw'
var layerIds = [
	'satellite-streets-v10',
	'light-v9',
]
var layerNames = [
	'Satellite',
	'Streets',
]
var center = [1.3417977, 103.9636011]
var zoom = 17
var attribution = 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>'
var layers = layerIds.map(layerId => {
	return L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/{style}/tiles/256/{z}/{x}/{y}?access_token={apiKey}',
		{
			maxZoom: 20,
			attribution: attribution,
			apiKey: apiKey,
			style: layerId,
		}
	)
})
var map = L.map('geojson-infobox', {layers: layers}).setView(
	center=center,
	zoom=zoom,
);
var baseMaps = {}
layers.map((layer, i) => {
	baseMaps[layerNames[i]] = layers[i]
})
L.control.layers(baseMaps).addTo(map);
var rectangle = {
	"type": "FeatureCollection",
	"features": [
		{
			"type": "Feature",
			"properties": {
				"stroke": "#555555",
				"stroke-width": 2,
				"stroke-opacity": 1,
				"fill": "#555555",
				"fill-opacity": 0.5,
				"number": 1
			},
			"geometry": {
				"type": "Polygon",
				"coordinates": [
					[
						[
							103.96244287490845,
							1.3407729100211627
						],
						[
							103.9633923768997,
							1.3407729100211627
						],
						[
							103.9633923768997,
							1.3413145678418545
						],
						[
							103.96244287490845,
							1.3413145678418545
						],
						[
							103.96244287490845,
							1.3407729100211627
						]
					]
				]
			}
		},
		{
			"type": "Feature",
			"properties": {
				"stroke": "#555555",
				"stroke-width": 2,
				"stroke-opacity": 1,
				"fill": "#555555",
				"fill-opacity": 0.5,
				"number": 2
			},
			"geometry": {
				"type": "Polygon",
				"coordinates": [
					[
						[
							103.96303832530975,
							1.3415103154406418
						],
						[
							103.96362572908401,
							1.3415103154406418
						],
						[
							103.96362572908401,
							1.3420761063572735
						],
						[
							103.96303832530975,
							1.3420761063572735
						],
						[
							103.96303832530975,
							1.3415103154406418
						]
					]
				]
			}
		}
	]
}
var geojson;
function highlightFeature(e) {
	var layer = e.target;
	layer.setStyle({
		weight: 10,
		fillOpacity: 1,
	});
	var infobox = document.getElementsByClassName('infobox')[0]
	infobox.innerHTML = '<b>Shape No.</b></br>'
	infobox.innerHTML += layer.feature.properties.number
}
function resetHighlight(e) {
	var layer = e.target;
	geojson.resetStyle(e.target);
}
function onEachFeature(feature, layer) {
	layer.on({
		mouseover: highlightFeature,
		mouseout: resetHighlight,
	});
}
var params = {
	style: {
		weight: 5,
		opacity: 1,
		color: '#ff0000',
		dashArray: '0',
		fillOpacity: 0.5,
		fillColor: '#0000ff',
	},
	onEachFeature: onEachFeature,
};
geojson = L.geoJson(rectangle, params).addTo(map);
var infobox = L.control({
	position: 'bottomright',
});
infobox.onAdd = function (map) {
	return L.DomUtil.create('div', 'infobox')
};
infobox.addTo(map);
</script>