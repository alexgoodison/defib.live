<script>
// @ts-nocheck
  import { onMount } from "svelte";
  import axios from "axios";
  import Location from "../components/location.svelte"
  import Footer from "../components/footer.svelte"

  var marker = {};

  const showResults = location => {
    marker = JSON.parse(location);
  }

  onMount(async() => {
    mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_KEY;
    
    var map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/alexg13/ckafu39hw024a1ioz3dxcw4iz',
      center: [-7.896901, 53.339954],
      zoom: 9,
      minZoom: 5,
      maxZoom: 18
    });


    // Fetch locations
    const { data: markers } = await axios.get(`${import.meta.env.VITE_API_BASE_URL}/all`)

    var geojson = [];

    markers.forEach(m => {
      var feature = {
        'type': 'Feature',
        'properties': {
          'marker': m
        },
        'geometry': {
          'type': 'Point',
          'coordinates': [m.latitude, m.longitude]
        }
      }

      geojson.push(feature);
    });

    map.addControl(
      new MapboxGeocoder({
        accessToken: mapboxgl.accessToken,
        mapboxgl: mapboxgl,
        countries: 'IE'
      })
    );

    map.addSource('markers', {
      'type': 'geojson',
      'data': {
        'type': 'FeatureCollection',
        'features': geojson
      }
    })

    map.addLayer({
      'id': 'markers',
      'type': 'symbol',
      'source': 'markers',
      'layout': {
        'icon-image': 'marker',
        'icon-allow-overlap': true
      }
    });

    map.addControl(new window.mapboxgl.NavigationControl());

    map.on('load', () => {
      map.loadImage(
        'https://img.icons8.com/office/40/000000/marker.png',
        (error, image) => {
          if (error) throw error;
          map.addImage('marker', image)
        }
      )
    });

    map.on('click', 'markers', e => {
      const data = e.features[0].properties.marker;
      showResults(data);
    });

    map.on('mousemove', 'markers', () => {
      map.getCanvas().style.cursor = 'pointer';
    });
      
    map.on('mouseleave', 'markers', () => {
      map.getCanvas().style.cursor = '';
    });
  })
</script>

<div class="h-screen w-screen grid md:grid-cols-2">
  <div class="h-screen flex flex-col gap-12 justify-between bg-gray-50">
    <!-- Header -->
    <div class="flex justify-between gap-4 items-center p-8">
      <div class="flex items-center">
        <img src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8">
        <span class="text-4xl font-bold">DEFIB</span>
        <span class="text-4xl">.live</span>
      </div>
      
      <a href="/suggest" class="font-medium text-gray-400 hover:text-red-500 duration-150 ease-in-out transition">Suggest a Defibrillator</a>
    </div>

    <!-- Content -->
    {#if Object.keys(marker).length > 0 }
      <Location location={marker} />
    {:else}
      <div class="px-8 mb-12">
        <div class="text-2xl text-center font-medium">Welcome to <span class="text-red-500">Defib.live</span>, Ireland's defibrillator locator.</div>
        <div class="text-xl text-center mb-4">Click on the markers to learn more about each defibrillator.</div>
      </div>
    {/if}

    <!-- Footer -->
    <div class="px-8 pt-8 pb-4">
      <Footer />
    </div>
  </div>

  <!-- Map -->
  <div class="min-h-screen bg-blue-500">
    <div id="map" />
  </div>
</div>

<style>
#map {
  width: 100%;
  height: 100%;
}
</style>