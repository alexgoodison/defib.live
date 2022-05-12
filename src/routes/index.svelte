<script>
// @ts-nocheck
  import { onMount } from "svelte";
  import axios from "axios";
  import Location from "../components/location.svelte"
  import Footer from "../components/footer.svelte"

  var marker = {};
  var isLoading = true;

  const showResults = location => {
    marker = JSON.parse(location);
  }

  onMount(async() => {
    try {
      // Fetch locations
      const { data: markers } = await axios.get(`${import.meta.env.VITE_API_BASE_URL}/all`);

      mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_KEY;
      
      var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/alexg13/cl2wcju8p000y14qcqx1fzj84',
        center: [-7.896901, 53.339954],
        zoom: 9,
        minZoom: 5,
        maxZoom: 18
      });

      var geojson = [];

      markers.forEach(m => {
        var feature = {
          'type': 'Feature',
          'properties': {
            'marker': m
          },
          'geometry': {
            'type': 'Point',
            'coordinates': [m.longitude, m.latitude]
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
      
      map.addControl(new window.mapboxgl.NavigationControl());
      
      map.on('load', () => {
        map.loadImage(
          'https://img.icons8.com/office/40/000000/marker.png',
          (error, image) => {
            if (error) throw error;
            
            map.addImage('marker', image)

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

            isLoading = false;
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
    } catch (error) {
      console.log(error);
    }
  })
</script>

<div class="h-screen w-screen grid md:grid-cols-2">
  <div class="h-screen flex flex-col gap-12 justify-between bg-gray-50">
    <!-- Header -->
    <div class="flex flex-wrap justify-between gap-4 items-center p-8">
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
  <div class="min-h-screen bg-gray-100 relative">
    <div class="h-screen">
      <div id="map" />
    </div>    

    {#if isLoading}
      <div class="absolute top-0 left-0 right-0 bottom-0 bg-gray-100">
        <div class="px-8 flex flex-col items-center justify-center h-full">
          <img class="heart h-48 w-48 object-contain" src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8">
          <div class="text-xl text-gray-400 font-semibold text-center">Loading Defibrillators...</div>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
#map {
  width: 100%;
  height: 100%;
}

.heart {
  animation: heartbeat 1.5s infinite;
}

@keyframes heartbeat
{
  0%
  {
    transform: scale( .75 );
  }
  20%
  {
    transform: scale( 1 );
  }
  40%
  {
    transform: scale( .75 );
  }
  60%
  {
    transform: scale( 1 );
  }
  80%
  {
    transform: scale( .75 );
  }
  100%
  {
    transform: scale( .75 );
  }
}
</style>