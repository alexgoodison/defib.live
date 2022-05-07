<script>
// @ts-nocheck
  import { onMount } from 'svelte';
  import tippy from 'tippy.js';
  import 'tippy.js/dist/tippy.css';
  import 'tippy.js/dist/svg-arrow.css';

  export let location = {};

  onMount(() => {
    tippy('#user-submitted-icon', {
      content: 'User submitted defibrillator', 
      placement: 'top', 
      delay: [null, null],
      arrow: tippy.roundArrow,
    });

    tippy('#flag-issue-icon', {
      content: 'Report an issue', 
      delay: [null, null], 
      placement: 'top', 
      arrow: tippy.roundArrow,
    });
  })
</script>

<div class="px-8">
  <div class="flex justify-between items-start gap-6 mb-6">
    <div class="text-4xl font-semibold text-red-500">{location.name}</div>

    <div class="flex gap-3 items-center">
      {#if location.is_user_submitted}
        <img id="user-submitted-icon" class="h-10" src="https://img.icons8.com/color/96/000000/customer-skin-type-7.png" alt="User submitted icon" />
      {/if}

      <a href={`/report?location=${location.name}`} id="flag-issue-icon" class="focus:outline-none">
        <img class="h-10" src="https://img.icons8.com/color/96/000000/filled-flag.png" alt="Flag issue icon" />
      </a>

      <div>
        <a target="_blank" href={`https://www.google.com/maps/dir/?api=1&destination=${location.latitude},${location.longitude}`} class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Get directions</a>
      </div>
    </div>
  </div>

  <div class="flex flex-col gap-8">
    <div>
      <div class="flex items-center gap-2 mb-1">
        <img class="h-6" src="https://img.icons8.com/color/96/000000/worldwide-location.png" alt="Locations icon" />
        <div class="text-xl font-semibold text-gray-800">Locations</div>
      </div>
      <div class="text-lg text-gray-600">{location.locations || 'No location data available'}</div>
    </div>

    <div>
      <div class="flex items-center gap-2 mb-1">
        <img class="h-6" src="https://img.icons8.com/color/96/000000/about-us-female.png" alt="Contact icon"/>
        <div class="text-xl font-semibold text-gray-800">Contact</div>
      </div>
      <div class="text-lg text-gray-600">{location.email || 'No contact email provided'}</div>
    </div>

    <div>
      <div class="flex items-center gap-2 mb-1">
        <img class="h-6" src="https://img.icons8.com/color/96/000000/clock--v1.png" alt="Clock icon" />
        <div class="text-xl font-semibold text-gray-800">Opening Hours</div>
      </div>
      <div class="text-lg text-gray-600">{location.opening_hours || 'No opening hours provided'}</div>
    </div>

    <div class="rounded-lg bg-gray-100 text-gray-600 flex px-3 py-2 gap-2 items-center">
      <img class="h-8 w-8" alt="Disclaimer icon" src="https://img.icons8.com/ios-glyphs/60/4b5563/disclaimer.png"/>
      <div>
        <div class="font-semibold">Disclaimer</div>
        <div class="text-sm">Please note that defibrillator locations are user submitted. We cannot be held liable for any location errors that may occur.</div>
      </div>
    </div>
  </div>
</div>