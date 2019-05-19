---
layout: post
title:	"Getting Started with Leaflet"
subtitle: "A tutorial for the Leaflet JS library."
date:	 2018-10-07 00:00:00 +0800
tags: code
comments: true
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.4/dist/leaflet.css" integrity="sha512-puBpdR0798OZvTTbP4A8Ix/l+A4dHDD0DGqYW6RQ+9jxkRFclaxxQb/SJAWZfWAkuyeQUytO7+7N4QKrDh+drA==" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.3.4/dist/leaflet.js" integrity="sha512-nMMmRyTVoLYqjP9hrbed9S+FzjZHW5gY1TWCHA5ckwXZBadntCNs8kEqAWdrb9O7rxbCaA4lKTIWjDXZxflOcA==" crossorigin="">
</script>
<style>
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

<div class='note note-left'>
<b>Note.</b> While using Leaflet, it will be best if you also sign up for an account with <a href="https://www.mapbox.com/">Mapbox</a>, which provides the layers for Leaflet. After signing up, go to the <a href="https://www.mapbox.com/account/">Account</a> page to get your API key, which will allow you to retrieve Mapbox maps and create custom maps. The following examples use a default tutorial API key, which you can substitute with your own.
</div>

Recently, while working on an assignment for my Urban Science Masters, I came across a wonderful JS library known as [Leaflet](https://leafletjs.com/). Here's a quickstart guide to using Leaflet, although there are also several [tutorials](https://leafletjs.com/examples.html) on the site.


*While I generally assume that the reader is familiar with basic HTML, CSS and Javascript, working files are provided for the newcomer. The files are all available at the accompany Github repository [here](https://github.com/greentfrapp/leaflet-tutorial). Just follow the instructions on the repo to access the files.*

---

## Get that Map

*This will follow the `1-basic.html` file in the Github repository. Source code [here](https://github.com/greentfrapp/leaflet-tutorial/blob/master/1-basic.html) and sample page [here](https://greentfrapp.github.io/leaflet-tutorial/1-basic.html)*

We start with displaying a simple map with Leaflet.

First of all, we need to include the CSS and JS files for Leaflet.

{% highlight html %}
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.4/dist/leaflet.css" integrity="sha512-puBpdR0798OZvTTbP4A8Ix/l+A4dHDD0DGqYW6RQ+9jxkRFclaxxQb/SJAWZfWAkuyeQUytO7+7N4QKrDh+drA==" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.3.4/dist/leaflet.js" integrity="sha512-nMMmRyTVoLYqjP9hrbed9S+FzjZHW5gY1TWCHA5ckwXZBadntCNs8kEqAWdrb9O7rxbCaA4lKTIWjDXZxflOcA==" crossorigin="">
</script>
{% endhighlight %}

Then we just define a `<div>` element that will contain our map:

{% highlight html %}
<div id='map'></div>
{% endhighlight %}

We will be creating a simple satellite map centered around my current university, Singapore University of Technology and Design. We will also be using the default `satellite-streets-v10` style from Mapbox. Check out the the full list of map styles [here](https://www.mapbox.com/api-documentation/#styles)!

All these details are included using the code below:

{% highlight javascript %}
// This should be the API key for your account when you sign up with Mapbox
var apiKey = 'pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw'
// Here we create the map layers - in this case, a satellite layer
var layerIds = [
	'satellite-streets-v10',
]
// This defines where the map is centered when the page is first shown
// NOTE! The coordinates are latitude first, then longitude second (latitudes should range from 0 to 90; longitudes should range from 180 to -180)
var center = [1.3417977, 103.9636011]
// This defines the zoom level of the map when the page is first shown
var zoom = 17
{% endhighlight %}

Next, we just create `L.tileLayer` objects and add that to a `L.map` object (`L` stands for Leaflet.

{% highlight javascript %}
// This actually adds the layer that we named above, as a tileLayer
var layers = layerIds.map(layerId => {
	return L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/{style}/tiles/256/{z}/{x}/{y}?access_token={apiKey}',
		{
			maxZoom: 20, // maximum zoom
			attribution: attribution,
			apiKey: apiKey,
			style: layerId,
		}
	)
})

// Here we create the map
// - we give it the layers
// - we tell it the center location
// - we decide how much to zoom-in
var map = L.map('map', {layers: layers}).setView(
	center=center,
	zoom=zoom,
);
{% endhighlight %}

The interactive map below should be displayed when you are done! You can also just open `1-basic.html` from the Github repo [here](https://github.com/greentfrapp/leaflet-tutorial).

<div id='basic' class='map'>
</div>

&nbsp;

You can try adjusting any of the variables mentioned above to customize the map that is displayed. Try changing the layer from `satellite-streets-v10` to `dark-v9` or `light-v9`!

## Add Multiple Layers

*This will follow the `2-layers.html` file in the Github repository. Source code [here](https://github.com/greentfrapp/leaflet-tutorial/blob/master/2-layers.html) and sample page [here](https://greentfrapp.github.io/leaflet-tutorial/2-layers.html)*

Adding multiple layers is really easy with Leaflet!

First we add another layer `light-v9` to the list of `layerIds` from earlier. Then, we create another list of `layerNames` to store the names of these layers that will show up in our interactive controls.

{% highlight javascript %}
var layerIds = [
	'satellite-streets-v10',
	'light-v9',
]
var layerNames = [
	'Satellite',
	'Streets',
]
{% endhighlight %}

Then add in some additional commands after the last line that creates our map, which will create the control menu.

{% highlight javascript %}
var baseMaps = {}
layers.map((layer, i) => {
	baseMaps[layerNames[i]] = layers[i]
})
L.control.layers(baseMaps).addTo(map);
{% endhighlight %}

TADA that's all!

<div id='multiple-layers' class='map'>
</div>

&nbsp;

Notice in the above map that the names of the layers in the control menu corresponds to the names we set in the `layerNames` list.

## Add Custom Shapes

*This will follow the `3-geojson.html` file in the Github repository. Source code [here](https://github.com/greentfrapp/leaflet-tutorial/blob/master/3-geojson.html) and sample page [here](https://greentfrapp.github.io/leaflet-tutorial/3-geojson.html)*

One new thing we will be introducing here is the web app at http://geojson.io. This helps to export any drawings on the map to a geojson format that is readable by Leaflet.

For starters, we can try drawing a rectangle near SUTD.

![Step 1](/assets/leaflet-tutorial/draw_1.png)

![Step 2](/assets/leaflet-tutorial/draw_2.png)

Then just copy the code on the right side and paste into our html file, prepending it with `var rectangle = `. See below for an example. See `3-geojson.html` in the repository for a clearer example.

{% highlight javascript %}
var rectangle = {
	"type": "FeatureCollection",
	"features": [
		{
			"type": "Feature",
			"properties": {},
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
		}
	]
}
{% endhighlight %}

Then we just need to add the following one-liner to add the geojson rectangle to our map.

{% highlight javascript %}
var geojson = L.geoJson(rectangle).addTo(map)
{% endhighlight %}

<div id='geojson' class='map'>
</div>

You can also customize the color and other properties of the geojson shape by adding additional parameters when initializing the `L.geoJson` object.

{% highlight javascript %}
var params = {
	style: {
		weight: 5, // border thickness
		opacity: 1, // border opacity
		color: '#ffffff', // border color
		fillOpacity: 0.5, // fill opacity
		fillColor: '#0000ff' // fill color
	}
}
var geojson = L.geoJson(rectangle, params).addTo(map)
{% endhighlight %}

<div id='geojson-params' class='map'>
</div>

&nbsp;

## Hover and Highlight Shapes

*This will follow the `4-highlight.html` file in the Github repository. Source code [here](https://github.com/greentfrapp/leaflet-tutorial/blob/master/4-highlight.html) and sample page [here](https://greentfrapp.github.io/leaflet-tutorial/4-highlight.html)*

To make the map feel more interactive, we can make a shape respond when the mouse hovers over it.

In the below code, the part where the shape responds is in the `highlightFeature` function. Here, we simply increase the border `weight` to 10 and increase the `fillOpacity` to 1.

{% highlight javascript %}
// Placeholder for L.geoJson object
var geojson;
// Function to highlight the object when the mouse hovers over the shape
function highlightFeature(e) {
	var layer = e.target;
	layer.setStyle({
		weight: 10, // Increase border weight to 10
		fillOpacity: 1, // Increase fill opacity to 1
	});
}
// Function to reset the highlight when the mouse moves away from the shape
function resetHighlight(e) {
	var layer = e.target;
	geojson.resetStyle(e.target);
}
// Applying the two functions above
function onEachFeature(feature, layer) {
	layer.on({
		mouseover: highlightFeature,
		mouseout: resetHighlight,
	});
}
// Adding the functions above to the parameters
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
{% endhighlight %}

<div id='geojson-highlight' class='map'>
</div>

&nbsp;

## Infobox

*This will follow the `5-infobox.html` file in the Github repository. Source code [here](https://github.com/greentfrapp/leaflet-tutorial/blob/master/5-infobox.html) and sample page [here](https://greentfrapp.github.io/leaflet-tutorial/5-infobox.html)*

We can also add an infobox at the bottom of the map, which changes content when the user hovers over specific shapes.

In order to illustrate this more effectively, let's add another rectangle using http://geojson.io. In addition, click on the original rectangle and add a new row in the Properties tab, named `number` with value `1`. Do the same for the new rectangle but with value `2`. See below for an example.

![Step 1](/assets/leaflet-tutorial/properties_1.png)

![Step 2](/assets/leaflet-tutorial/properties_2.png)

Next just copy the geojson code over as per before, prepending it with `var rectangle = `, using it to replace the previous geojson code.

Then add in the following lines of code to add the infobox in the bottom right corner of the map.

Add this to the CSS:

{% highlight css %}
.infobox {
	padding: 8px;
	box-shadow: 0 0 15px rgba(0,0,0,0.2);
	border-radius: 8px;
	font-size: 18px;
}
{% endhighlight %}

And this to the end of the Javascript:

{% highlight javascript %}
var infobox = L.control({
	position: 'bottomright',
});
infobox.onAdd = function (map) {
	return L.DomUtil.create('div', 'infobox');
};
infobox.addTo(map);
{% endhighlight %}

Finally, we just need to amend the `highlightFeature` function to get the map to update the infobox when we hover over the shape. Recall that this function is called whenever we hover over a shape.

We update the HTML of the infobox using the `number` property that we added into the geojson earlier, which we access using `layer.feature.properties.number`.

{% highlight javascript %}
function highlightFeature(e) {
	var layer = e.target;
	layer.setStyle({
		weight: 10,
		fillOpacity: 1,
	});
	// New lines below
	// We get the `infobox` element and then update the HTML inside the element using the shape's `number` property that we defined earlier.
	var infobox = document.getElementsByClassName('infobox')[0]
	infobox.innerHTML = '<b>Shape No.</b></br>'
	infobox.innerHTML += layer.feature.properties.number
}
{% endhighlight %}

Then we should have something that looks like the map below! Hovering over the two shapes should change the infobox content.

This also means that you can change what the infobox says by editing the `infobox.innerHTML` part in the code above.

In addition, you can assign and access different properties of each shape (such as different types of buildings) using the geojson.io webapp, by changing the `number` property to something else, say `buildingType`, which will be accessed by `layer.feature.properties.buildingType` in the code.

---

That's it for the tutorial! Hope it helped!

<div id='geojson-infobox' class='map'>
</div>

&nbsp;

## Bonus!

If you did sign up for a [Mapbox](https://www.mapbox.com/) account, you will also have access to [Mapbox Studio](https://www.mapbox.com/studio/), which allows you to create your own map styles and also browse a gallery of gorgeous styles. Check out some examples below!

![Mapbox gallery style 1](/assets/leaflet-tutorial/mapbox_1.png)
![Mapbox gallery style 2](/assets/leaflet-tutorial/mapbox_2.png)
![Mapbox gallery style 3](/assets/leaflet-tutorial/mapbox_3.png)

More on this next time!

<script>
// Code for #basic
var apiKey = 'pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw'
var layerIds = [
	'satellite-streets-v10',
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
var map = L.map('basic', {layers: layers}).setView(
	center=center,
	zoom=zoom,
);
</script>

<script>
// Code for #geojson
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
var map = L.map('multiple-layers', {layers: layers}).setView(
	center=center,
	zoom=zoom,
);
var baseMaps = {}
layers.map((layer, i) => {
	baseMaps[layerNames[i]] = layers[i]
})
L.control.layers(baseMaps).addTo(map);
</script>
<script>
// Code for #geojson
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
var map = L.map('geojson', {layers: layers}).setView(
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
			"properties": {},
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
		}
	]
}
var geojson = L.geoJson(rectangle).addTo(map)
</script>
<script>
// Code for #geojson-params
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
var map = L.map('geojson-params', {layers: layers}).setView(
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
			"properties": {},
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
		}
	]
}
var params = {
	style: {
		weight: 5,
		opacity: 1,
		color: '#f00',
		dashArray: '0',
		fillOpacity: 0.5,
		fillColor: '#00f',
	}
}
var geojson = L.geoJson(rectangle, params).addTo(map)
</script>
<script>
// Code for #geojson-highlight
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
var map = L.map('geojson-highlight', {layers: layers}).setView(
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
			"properties": {},
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
</script>
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
