
<script>
// @ts-nocheck
  import axios from "axios";
  import Footer from "../components/footer.svelte";

  let form = {
    email: null,
    subject: null,
    details: null
  }

  let errors = [];

  const submitForm = () => {
    errors = [];

    axios.post(`${import.meta.env.VITE_API_BASE_URL}/v1/report`, form)
      .then(() => window.location = "/report_success")
      .catch(() => errors.push("An unexpected error occurred. Please try again later."))
  }
</script>

<a href="/" class="flex items-center p-8">
  <img src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8" />
  <span class="text-4xl font-bold">DEFIB</span>
  <span class="text-4xl">.live</span>
</a>

<form on:submit|preventDefault={submitForm} class="max-w-4xl mx-auto px-4 flex flex-col gap-8">
  <div class="font-bold text-3xl text-gray-800">
    Report an Issue
  </div>

  <div>
    <div class="mb-2 text-lg">Please provide your email so we can contact you.</div>
    <input required type="email" placeholder="your@email.com" name="email" id="email" bind:value={form.email}>
  </div>

  <div>
    <div class="mb-2 text-lg">If the issue is in relation to a defibrillator, please let us know which one it is.</div>
    <input type="text" placeholder="Input location name (example: Ballyhea GAA Club)" name="subject" id="subject" bind:value={form.subject}>
  </div>

  <div>
    <div class="mb-2 text-lg">Provide details on what the issue is that you are reporting.</div>
    <textarea type="text" rows="7" required placeholder="Enter report details" name="locations" id="locations" bind:value={form.details} />
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
    <button type="submit" class="rounded-lg px-3 py-2 bg-blue-600 hover:bg-blue-700 transition ease-in-out duration-150 text-white font-semibold">Submit</button>
    <a href="/" class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Back to map</a>
  </div>
</form>

<div class="flex justify-center pt-16 px-8 pb-4">
  <Footer />
</div>

<style>
  input, textarea {
    @apply border border-gray-100 w-full bg-white shadow-sm text-lg rounded-lg px-4 py-2;
  }
</style>
