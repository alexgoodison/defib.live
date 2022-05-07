<script>
  import Footer from "../components/footer.svelte";
</script>

<div class="flex flex-col justify-between h-screen gap-12">
  <a href="/" class="flex items-center p-8">
    <img src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8" />
    <span class="text-4xl font-bold">DEFIB</span>
    <span class="text-4xl">.live</span>
  </a>
  
  <div class="max-w-3xl mx-auto px-8 text-center">
    <div class="text-red-500 font-bold text-5xl mb-4">Success!</div>
    <div class="text-2xl mb-8">Your defibrillator location was submitted successfully! We'll keep you updated via email.</div>
    <div class="flex gap-4 justify-center">
      <a href="/" class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Back to map</a>
      <a href="/suggest" class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Suggest another defibrillator</a>
    </div>
  </div>  

  <div class="flex justify-center pt-24 p-8">
    <Footer />
  </div>
</div>