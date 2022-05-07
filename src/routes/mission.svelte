<script>
  import Footer from "../components/footer.svelte";
</script>

<a href="/" class="flex items-center p-8">
  <img src="heart.svg" alt="Heart Icon" title="Icon provided by ICONS8" />
  <span class="text-4xl font-bold">DEFIB</span>
  <span class="text-4xl">.live</span>
</a>

<div class="max-w-5xl px-4 mx-auto flex flex-col gap-8">
  <div class="flex gap-8 justify-between">
    <div class="font-bold text-3xl text-gray-800">
      My Mission
    </div>

    <a href="/" class="rounded-lg px-3 py-2 bg-gray-200 text-gray-600 font-semibold hover:bg-gray-300 transition ease-in-out duration-150">Back to map</a>
  </div>

  <div class="text-lg">
    Defib.live was created out of necessity. The island of Ireland needed a comprehensive, user-friendly defibrillator map that had the potential to become a  household name across the country. My sole intention for this website, is to never leave anyone asking themselves, where is my nearest defibrillator?
  </div>

  <div class="text-lg">
    Unfortunately, a lot of us may find ourselves in need of a defibrillator and not know where our closest one is. When someone suddenly suffers cardiac arrest, every minute counts when it comes to saving their life. 
  </div>

  <div class="text-lg">
    My aim for defib.live is to eventually turn it into a global platform that everyone can turn to whether you're at home or on holidays. If you are aware of any defibrillator locations that are not on the map, I encourage you to <a class="text-blue-500 font-semibold" href="/suggest">submit them to be added</a>. In reality, this may have started as my own website, but I want this to be a global project with worldwide impact. If you have any feature ideas or even if you just want to talk, dont hesitate to email me at alexandergoodison@gmail.com.
  </div>

  <div class="font-semibold text-lg">
    Alex Goodison
  </div>
  
  <div>
    Made with<span class="px-1">❤️</span> in Co. Cork
  </div>
</div>

<div class="flex justify-center pt-16 px-8 pb-4">
  <Footer />
</div>