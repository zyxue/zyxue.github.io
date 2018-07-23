---
layout: page
title: About
permalink: /about/
---

This is Zhuyi Xue's blog. I may put some programming experiments here, too.

Below is a map showing a bicycle trip (<span style="color:red;">red
track</span>) I did in January, 2009. It took me 11 days to ride 1400km,
crossing 40 cities and five provinces from northern to southern China.

<button id="button-trip-map">click to see the map</button>

<img id="trip-map" src="/assets/beijing2wenzhou_trip.jpg" width="550" alt="trip photo" style="display:none;">

<script>
$('#button-trip-map').click(function() {$('img#trip-map').toggle()})
</script>

{% include footer.html %}
