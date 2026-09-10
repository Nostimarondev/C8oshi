<template>
  <footer class="bg-black text-white pt-20 overflow-hidden border-t border-gray-800">
    <div class="max-w-[90rem] mx-auto px-6 md:px-12">
      <!-- Top Section: 4 Columns -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 lg:gap-8 mb-16">
        
        <!-- Column 1: Brand -->
        <div class="flex flex-col gap-6">
          <div class="flex items-center">
            <div class="flex flex-col uppercase leading-none">
              <span class="font-black tracking-widest text-lg">C8OSHI</span>
              <span class="text-[0.65rem] tracking-widest text-gray-400 font-bold">NAKAM8TO</span>
            </div>
          </div>
        </div>

        <!-- Column 2: EXPLORE -->
        <div class="flex flex-col gap-6">
          <h3 class="uppercase font-bold tracking-widest text-sm border-b border-gray-800 pb-2">Explore</h3>
          <ul class="flex flex-col gap-4 text-sm text-gray-400 font-medium">
            <li><a href="#lore-section" class="hover:text-white transition-colors">The Lore</a></li>
            <li><a href="#gallery-section" class="hover:text-white transition-colors">Wall of F8me</a></li>
            <li><a href="#" class="hover:text-white transition-colors">Tokenomics</a></li>
          </ul>
        </div>

        <!-- Column 3: COMMUNITY -->
        <div class="flex flex-col gap-6">
          <h3 class="uppercase font-bold tracking-widest text-sm border-b border-gray-800 pb-2">Community</h3>
          <ul class="flex flex-col gap-4 text-sm text-gray-400 font-medium">
            <li><a href="#" class="hover:text-white transition-colors">X / Twitter</a></li>
            <li><a href="#" class="hover:text-white transition-colors">Telegram</a></li>
            <li><a href="https://c8ntinuum.com" target="_blank" class="hover:text-white transition-colors">C8ntinuum</a></li>
          </ul>
        </div>

        <!-- Column 4: TRADE -->
        <div class="flex flex-col gap-6">
          <h3 class="uppercase font-bold tracking-widest text-sm border-b border-gray-800 pb-2">Trade</h3>
          <ul class="flex flex-col gap-4 text-sm text-gray-400 font-medium">
            <li><a href="#" class="hover:text-white transition-colors">Dexscreener</a></li>
            <li><a href="#" class="hover:text-white transition-colors">Buy $C8</a></li>
            <li><a href="#" class="hover:text-white transition-colors">Contract Info</a></li>
          </ul>
        </div>

      </div>

      <!-- Divider -->
      <div class="h-px w-full bg-gray-800 mb-8"></div>

      <!-- Middle Section: Terms & Socials -->
      <div class="flex flex-col md:flex-row justify-between items-start md:items-start gap-8 mb-12">
        <!-- Left Side: Links & Copyright -->
        <div class="flex flex-col gap-6">
          <div class="flex flex-wrap items-center gap-6 text-xs text-gray-400 font-bold tracking-widest">
            <a href="#" class="hover:text-white transition-colors">Terms & Conditions</a>
            <a href="#" class="hover:text-white transition-colors">Privacy Policy</a>
            <a href="#" class="hover:text-white transition-colors">Terms of Service</a>
          </div>
          <div class="text-xs text-gray-600 font-bold tracking-widest">
            &copy; {{ new Date().getFullYear() }} C8OSHI. All rights reserved.
          </div>
        </div>

        <!-- Right Side: Social Icons -->
        <div class="flex items-center gap-4 text-gray-400">
          <a href="#" class="hover:text-white transition-colors">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
              <path d="M13.3174 10.4357L21.4395 1H19.5165L12.4691 9.19524L6.84079 1H0.339844L8.85764 13.3854L0.339844 23.2917H2.26284L9.7042 14.6366L15.6321 23.2917H22.133L13.317 10.4357H13.3174ZM10.6766 13.5049L9.81861 12.2778L2.9554 2.46467H5.90802L11.4552 10.4005L12.3132 11.6276L19.5173 21.9056H16.5647L10.6766 13.5054V13.5049Z" />
            </svg>
          </a>
          <!-- Discord Icon -->
          <a href="#" class="hover:text-white transition-colors">
            <svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
              <path d="M20.317 4.36999C18.7915 3.66827 17.1593 3.1491 15.4529 2.84654C15.4223 2.84092 15.3908 2.8559 15.3752 2.88371C15.166 3.25624 14.9288 3.73801 14.7649 4.13117C12.9234 3.85521 11.0911 3.85521 9.27734 4.13117C9.11333 3.72877 8.86877 3.25624 8.65787 2.88371C8.64235 2.85501 8.61081 2.84002 8.58021 2.84654C6.87383 3.1491 5.24171 3.66827 3.71609 4.36999C3.69614 4.37916 3.68112 4.39498 3.67198 4.41405C0.584145 9.03457 -0.32049 13.5381 0.0963351 17.962C0.0984852 17.9868 0.111867 18.0097 0.131754 18.0246C2.18181 19.5312 4.17062 20.4429 6.11586 21.0506C6.14713 21.0604 6.1804 21.0487 6.19835 21.0249C6.66258 20.392 7.07823 19.7214 7.4384 19.0205C7.45869 18.9814 7.44104 18.9344 7.40028 18.9192C6.76451 18.6775 6.1558 18.384 5.57945 18.046C5.53483 18.0192 5.53162 17.9542 5.5732 17.9233C5.69837 17.8286 5.82361 17.7303 5.94273 17.632C5.96495 17.6139 5.99614 17.6101 6.02131 17.6214C9.91498 19.3989 14.1506 19.3989 17.9943 17.6214C18.0203 17.6094 18.0514 17.6139 18.0736 17.632C18.1928 17.7312 18.3179 17.8286 18.4447 17.9233C18.4862 17.9551 18.4822 18.02 18.4376 18.046C17.8596 18.3905 17.2483 18.6852 16.609 18.9184C16.5682 18.9335 16.5513 18.9814 16.5716 19.0205C16.9388 19.7206 17.3553 20.3912 17.817 21.0241C17.8349 21.0487 17.8682 21.0604 17.8995 21.0498C19.8517 20.4421 21.8414 19.5312 23.8906 18.0246C23.9114 18.0097 23.9248 17.9868 23.9269 17.962C24.4082 12.8715 23.0135 8.40698 20.3444 4.41405C20.3344 4.39498 20.3194 4.37916 20.317 4.36999ZM8.01997 15.3094C6.84561 15.3094 5.8797 14.2255 5.8797 12.8943C5.8797 11.5632 6.82522 10.4792 8.01997 10.4792C9.22485 10.4792 10.1804 11.5732 10.1601 12.8943C10.1601 14.2255 9.21469 15.3094 8.01997 15.3094ZM16.0305 15.3094C14.8562 15.3094 13.8902 14.2255 13.8902 12.8943C13.8902 11.5632 14.8357 10.4792 16.0305 10.4792C17.2354 10.4792 18.1909 11.5732 18.1706 12.8943C18.1706 14.2255 17.2252 15.3094 16.0305 15.3094Z"/>
            </svg>
          </a>
          <!-- Telegram Icon -->
          <a href="#" class="hover:text-white transition-colors">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
              <path d="M22.0504 2.89481C22.0504 2.89481 24.1507 2.05284 23.9406 4.05269C23.8356 5.10526 22.8905 10.6835 22.3653 15.9461L21.1051 21.6295C21.1051 21.6295 20.895 22.8925 19.7398 23.2083C18.5846 23.524 16.7993 22.3662 16.4842 22.1557L11.0232 17.6298L8.60773 19.9452C8.60773 19.9452 8.18765 20.3662 7.76757 20.3662V16.7878L18.4796 7.10499C18.4796 7.10499 19.4248 6.26298 17.6394 7.52595L6.61234 14.8933H2.09647C2.09647 14.8933 0.626194 14.7881 0.101037 13.946C-0.319047 13.2093 0.941258 12.683 0.941258 12.683L20.5801 5.10512C20.5801 5.10512 22.0504 4.47363 22.0504 2.89481Z"/>
            </svg>
          </a>
        </div>
      </div>
    </div>

    <!-- Bottom Section: Gigantic Text Hollow -->
    <div class="w-full flex justify-center items-end overflow-hidden leading-none select-none px-6 md:px-12 mt-12 pb-4">
      <!-- We use CSS text-stroke to make it hollow and dark gray -->
      <h1 class="text-[20vw] md:text-[25vw] font-black tracking-tighter uppercase m-0 p-0 leading-[0.75]" style="color: transparent; -webkit-text-stroke: 2px #222222;">
        C8OSHI
      </h1>
    </div>
  </footer>
</template>
