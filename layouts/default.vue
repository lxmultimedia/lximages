<template>
  <div class="bg-black">
    <header
      class="z-20 flex justify-between items-center bg-gray-800 sticky top-0 w-full h-15"
    >
      <nav>
        <a href="/" class="flex space-x-4 items-center p-4 text-xl font-bold"
          ><img
            src="assets/images/icon-picture-5.jpg"
            alt="Logo"
            class="w-10"
          /><span>LX Image Search</span></a
        >
      </nav>
    </header>

    <Nuxt />

    <footer
      id="footer"
      class="mt-5 p-4 flex justify-between items-center text-center bg-gray-800 w-full"
    >
      <div>
        &copy; 2021 -
        <a
          target="_blank"
          href="https://www.lxmedia.tk"
          class="underline underline-offset-8 hover.underline-offset-4 text-yellow-500"
          >LX Multimedia</a
        >
      </div>
      <div class="flex space-x-4">
        <span>Powered by</span>
        <a href="https://www.pixabay.com" target="__blank"
          ><img
            class="w-40 m-auto"
            src="assets/images/Pixabay-logo.svg.png"
            alt="Pixabay logo"
        /></a>
      </div>
    </footer>
  </div>
</template>
