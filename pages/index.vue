<script setup>
import { computed, onMounted, ref } from "vue";

const monitorData = ref(null)

onMounted(async () => {
  monitorData.value = await $fetch('/api/latest-value').catch(() => null)

  // Initialize AOS after the component is mounted
  if (process.client) {
    import('aos').then((AOS) => {
      AOS.init({
        duration: 800,        // Animation duration
        easing: 'ease-in-out', // Animation easing
        once: false,          // Whether animation should happen only once
        mirror: true,         // Whether elements should animate out while scrolling past them
        offset: 50,          // Offset (in px) from the original trigger point
      });
    });
  }
})

const trafficDateReadable = computed(() => {
  if (!monitorData.value?.taken_at) return 'Loading...'
  const dateObj = new Date(monitorData.value.taken_at)
  return dateObj.toLocaleString()
})
console.log(monitorData)
</script>

<template>
  <div>
    <!--- Jumbotron ---->
    <section class="relative min-h-screen flex items-center justify-center bg-cover bg-center bg-no-repeat overflow-hidden"
             style="background-image: url('/traffic.png');">
      <!-- Overlay gelap -->
      <div class="absolute inset-0 bg-black bg-opacity-80"></div>

      <!-- Konten teks -->
      <div class="relative z-10 py-4 px-4 mx-auto max-w-screen-xl text-center lg:py-8">
        <h1 data-aos="fade-down" data-aos-delay="200" class="mb-4 text-4xl font-extrabold tracking-tight leading-none text-white md:text-5xl lg:text-6xl">
          Arial Project <br/> Capstone Project Teknik Elektro
        </h1>
        <p data-aos="fade-up" data-aos-delay="400" class="mb-8 text-lg font-normal text-gray-200 lg:text-xl sm:px-16 lg:px-48">
          Kami coba untuk mengatasi masalah kemacetan di Bali dengan menggunakan Artificial Intelligence dengan kontrol lampu merah adaptif yang
          ditenagai oleh pembangkit listrik tenaga surya
        </p>
        <div data-aos="zoom-in" data-aos-delay="600" class="flex flex-col space-y-4 sm:flex-row sm:justify-center sm:space-y-0">
          <a href="#"
             class="inline-flex justify-center items-center py-3 px-5 text-base font-medium text-center text-white rounded-lg bg-red-600 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300">
            Cek tingkat kemacetan
            <svg class="w-3.5 h-3.5 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                 fill="none" viewBox="0 0 14 10">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M1 5h12m0 0L9 1m4 4L9 9"/>
            </svg>
          </a>
          <a href="#"
             class="py-3 px-5 sm:ms-4 text-sm font-medium text-white focus:outline-none bg-transparent rounded-lg border border-white hover:bg-white hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-white">
            Tentang kami
          </a>
        </div>
      </div>
    </section>
    <!--- End of Jumbotron ---->

    <section class="relative min-h-screen flex flex-col md:flex-row bg-gradient-to-br from-gray-600 to-gray-900">
      <!-- Left Section -->
      <div data-aos="fade-right" data-aos-duration="1000" class="w-full md:w-1/2 flex bg-gradient-to-br flex-col from-gray-800 to-gray-700">
        <!-- Top Left -->
        <div class="h-1/3 md:h-1/2 flex flex-col items-center justify-center p-6 md:p-8 bg-gradient-to-r ">
          <h3 data-aos="fade-up" data-aos-delay="200" class="text-white text-xl font-medium mb-4">Lokasi Pengamatan</h3>
          <figure data-aos="flip-left" data-aos-delay="400"
                  class="w-full max-w-md overflow-hidden rounded-xl shadow-2xl transform transition-transform hover:scale-102">
            <div class="relative">
              <img :src="monitorData?.image" alt="Gambar Kemacetan" class="w-full h-auto object-cover max-w-full">
              <div class="absolute inset-0 bg-black bg-opacity-20 hover:bg-opacity-10 transition-all"></div>
            </div>
            <figcaption class="p-3 bg-gray-900 text-white text-sm">
              <div class="flex items-center">
                <i class="fas fa-map-marker-alt mr-2 text-red-400"></i>
                <span>Kemacetan di Jl Bypass Ngurah Rai Jimbaran</span>
              </div>
            </figcaption>
          </figure>
        </div>

        <!-- Bottom Left -->
        <div class="h-1/3 md:h-1/2 flex items-center justify-center order-3 md:order-2 p-6 md:p-8 bg-gradient-to-r ">
          <div class="w-full max-w-md">
            <h3 data-aos="fade-up" data-aos-delay="200" class="text-white text-xl font-medium mb-4">Data Statistik</h3>
            <figure data-aos="flip-right" data-aos-delay="400" class="overflow-hidden rounded-xl shadow-2xl transform transition-transform hover:scale-102">
              <div class="relative">
                <img src="/placeholder.webp" alt="Data Kemacetan" class="w-full h-auto object-cover">
                <div class="absolute inset-0 bg-black bg-opacity-20 hover:bg-opacity-10 transition-all"></div>
              </div>
              <figcaption class="p-3 bg-gray-900 text-white text-sm">
                <div class="flex items-center">
                  <i class="fas fa-chart-line mr-2 text-green-400"></i>
                  <span>Grafik Data Kemacetan Harian</span>
                </div>
              </figcaption>
            </figure>
          </div>
        </div>
      </div>

      <!-- Right Section -->
      <div data-aos="fade-left" data-aos-duration="1000"
           class="w-full md:w-1/2 h-1/3 md:h-auto flex flex-col items-start justify-center order-2 md:order-3 p-6 md:p-10">
        <div
            class="p-6 md:p-10 h-full w-full bg-gradient-to-br from-white to-gray-300 rounded-2xl shadow-xl border-l-4 border-gray-600">
          <!-- Header section -->
          <div data-aos="fade-down" data-aos-delay="300" class="flex items-center mb-8">
            <div class="w-2 h-12 bg-yellow-600 mr-4 rounded-full"></div>
            <div>
              <h2 class="text-2xl md:text-3xl lg:text-4xl text-gray-800 font-bold tracking-tight">Tingkat Kemacetan</h2>
              <p class="text-sm text-gray-500 mt-1">Analisis data terkini</p>
            </div>
          </div>

          <!-- Location section -->
          <div data-aos="fade-up" data-aos-delay="400" class="mb-8 pb-6 border-b border-gray-200">
            <div class="flex items-start">
              <div class="bg-blue-50 p-2 rounded-lg mr-4">
                <i class="fas fa-map-marked-alt text-blue-600 text-xl"></i>
              </div>
              <div>
                <h3 class="text-lg font-medium text-gray-800 mb-1">Lokasi Observasi</h3>
                <p class="text-md md:text-lg text-gray-700">
                  <span class="font-medium">Jl. By Pass Ngurah Rai</span> arah Taman Griya ke Denpasar.
                </p>
              </div>
            </div>
          </div>

          <!-- Data information section -->
          <div class="space-y-6">
            <!-- Data collection time -->
            <div data-aos="fade-right" data-aos-delay="500" class="flex items-start">
              <div class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center mr-4">
                <i class="fas fa-clock text-blue-600"></i>
              </div>
              <div>
                <h4 class="text-lg font-medium text-gray-800">Data diambil pada</h4>
                <p class="text-gray-600 bg-gray-100 px-3 py-1 rounded-md mt-1 inline-block">
                  {{ trafficDateReadable || "Data tidak tersedia.." }}
                </p>
              </div>
            </div>

            <!-- Traffic density -->
            <div data-aos="fade-right" data-aos-delay="600" class="flex items-start">
              <div class="w-10 h-10 rounded-full bg-red-100 flex items-center justify-center mr-4">
                <i class="fas fa-car-crash text-red-600"></i>
              </div>
              <div>
                <h4 class="text-lg font-medium text-gray-800">Tingkat Kepadatan</h4>
                <p class="text-gray-600 bg-gray-100 px-3 py-1 rounded-md mt-1 inline-block">
                  {{ monitorData?.value || "Tingkat kemacetan tidak tersedia.." }}
                </p>
              </div>
            </div>

            <!-- Vehicle counts -->
            <div data-aos="fade-right" data-aos-delay="700" class="flex items-start">
              <div class="w-10 h-10 rounded-full bg-green-100 flex items-center justify-center mr-4 mt-1">
                <i class="fas fa-route text-green-600"></i>
              </div>
              <div class="w-full">
                <h4 class="text-lg font-medium text-gray-800 mb-2">Detail Jumlah Kendaraan</h4>
                <div class="bg-gray-50 rounded-lg p-4 w-full">
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    <div data-aos="zoom-in" data-aos-delay="800" class="flex items-center space-x-3 p-2 bg-white rounded-md shadow-sm">
                      <i class="fas fa-motorcycle text-gray-500"></i>
                      <div>
                        <div class="text-xs text-gray-500">Motor</div>
                        <div class="font-medium">{{ monitorData?.motorcycle_count || 0 }}</div>
                      </div>
                    </div>

                    <div data-aos="zoom-in" data-aos-delay="900" class="flex items-center space-x-3 p-2 bg-white rounded-md shadow-sm">
                      <i class="fas fa-car text-gray-500"></i>
                      <div>
                        <div class="text-xs text-gray-500">Mobil</div>
                        <div class="font-medium">{{ monitorData?.car_count ||  0 }}</div>
                      </div>
                    </div>

                    <div data-aos="zoom-in" data-aos-delay="1000" class="flex items-center space-x-3 p-2 bg-white rounded-md shadow-sm">
                      <i class="fas fa-truck-pickup text-gray-500"></i>
                      <div>
                        <div class="text-xs text-gray-500">Truck Kecil</div>
                        <div class="font-medium">{{ monitorData?.smalltruck_count || 0 }}</div>
                      </div>
                    </div>

                    <div data-aos="zoom-in" data-aos-delay="1100" class="flex items-center space-x-3 p-2 bg-white rounded-md shadow-sm">
                      <i class="fas fa-truck text-gray-500"></i>
                      <div>
                        <div class="text-xs text-gray-500">Kendaraan Besar</div>
                        <div class="font-medium">{{ monitorData?.bigvehicle_count || 0  }}</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- Last updated badge -->
            <div data-aos="fade-up" data-aos-delay="1200" class="mt-4 text-right">
              <div class="inline-flex items-center text-xs text-gray-500 bg-gray-100 px-3 py-1 rounded-full">
                <i class="fas fa-sync-alt mr-1"></i> Terakhir diperbarui: {{ trafficDateReadable }}
              </div>
            </div>
          </div>
        </div>
      </div>

    </section>

  </div>
</template>

<style scoped>
/* Add custom styles for AOS animations if needed */
[data-aos] {
  pointer-events: none; /* Mencegah konflik touch di iOS */
  transform: translateZ(0); /* Aktifkan hardware acceleration */
}
@supports (-webkit-touch-callout: none) {
  body {
    -webkit-overflow-scrolling: touch;
  }
}
</style>