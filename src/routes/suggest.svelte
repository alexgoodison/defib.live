<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import axios from "axios";
  import Footer from "../components/footer.svelte";

  let form = {
    name: null,
    submission_email: null,
    locations: null,
    latitude: null,
    longitude: null
  }

  let errors = [];

  onMount(async () => {
    mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_KEY;

    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/alexg13/ckafu39hw024a1ioz3dxcw4iz",
      center: [-7.896901, 53.339954],
      zoom: 9,
      minZoom: 5,
      maxZoom: 18,
    });

    map.addControl(new window.mapboxgl.NavigationControl());

    var marker = new window.mapboxgl.Marker({
      draggable: true,
    })
      .setLngLat([-7.896901, 53.339954])
      .addTo(map);

    marker.on("dragend", () => {
      const c = marker.getLngLat();
      form.latitude = c.lat;
      form.longitude = c.lng;
    });
  });

  const submitForm = () => {
    errors = [];

    if (form.latitude == null || form.longitude == null) {
      errors.push("Please drag the blue marker on the map to the location of the defibrillator.")
      return;
    }

    axios.post(`${import.meta.env.VITE_API_BASE_URL}/submit`, form)
      .then(() => window.location = "/suggest_success")
      .catch(() => errors.push("An unexpected error occurred. Please try again later."))
  }
</script>

<a href="/" class="flex items-center p-8">
  <img src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8" />
  <span class="text-4xl font-bold">DEFIB</span>
  <span class="text-4xl">.live</span>
</a>

<div class="container px-4 mx-auto grid grid-cols-2 gap-12">
  <form on:submit|preventDefault={submitForm} class="flex flex-col gap-8">
    <div class="font-bold text-3xl text-gray-800">
      Suggest a Defibrillator
    </div>
    
    <div>
      <div class="mb-2 text-lg">What is the name of this location?</div>
      <input required type="text" placeholder="e.g. John's Pizzeria" name="name" id="name" bind:value={form.name}>
    </div>

    <div>
      <div class="mb-2 text-lg">Provide specific location(s) of where someone can find the defibrillator.</div>
      <textarea type="text" required placeholder="e.g. beside the lifts on the ground floor" name="locations" id="locations" bind:value={form.locations} />
    </div>

    <div>
      <div class="mb-2 text-lg">Please provide your email so we can contact you if necessary.</div>
      <input required type="email" placeholder="your@email.com" name="submission_email" id="submission_email" bind:value={form.submission_email}>
    </div>

    {#if errors.length > 0}
      <div class="bg-gray-200 text-gray-800 py-3 px-4 rounded-lg">
        <div class="text-lg font-semibold">Errors:</div>
        {#each errors as error}
          <ul>
            <li>{error}</li>
          </ul>
        {/each}
      </div>
    {/if}
    
    <div class="flex gap-2">
      <button type="submit" class="rounded-lg px-3 py-2 bg-blue-600 hover:bg-blue-700 transition ease-in-out duration-150 text-white font-semibold">Submit defibrillator</button>
      <a href="/" class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Back to map</a>
    </div>
  </form>

  <div>
    <div class="mb-2 text-lg">Drag the marker to the location of the defibrillator on the map.</div>
    <div id="map" />
  </div>
</div>

<div class="flex justify-center pt-16 px-8 pb-4">
  <Footer />
</div>

<style>
  #map {
    width: 100%;
    height: 70vh;
    border-radius: 12px;
  }

  input, textarea {
    @apply border border-gray-100 w-full bg-white shadow-sm text-lg rounded-lg px-4 py-2;
  }
</style>
