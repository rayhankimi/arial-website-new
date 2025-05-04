<script setup>
import { computed, onMounted, ref } from "vue";

const monitorData = ref(null);
const isAboutModalOpen = ref(false);

onMounted(async () => {
  monitorData.value = await $fetch('/api/latest-value').catch(() => null)

  if (process.client) {
    import('aos').then((AOS) => {
      AOS.init({
        duration: 800,
        easing: 'ease-in-out',
        once: false,
        mirror: true,
        offset: 50,
      });
    });
  }
})

const scrollToSection = (id) => {
  const element = document.getElementById(id);
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  }
};

const openAboutModal = () => {
  isAboutModalOpen.value = true;
  document.body.style.overflow = 'hidden';
};

const closeAboutModal = () => {
  isAboutModalOpen.value = false;
  document.body.style.overflow = 'auto';
};

const trafficDateReadable = computed(() => {
  if (!monitorData.value?.taken_at) return 'Loading...'
  const dateObj = new Date(monitorData.value.taken_at)
  return dateObj.toLocaleString()
})
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
        <div class="flex flex-col space-y-4 sm:flex-row sm:justify-center sm:space-y-0">
          <a @click="scrollToSection('traffic-data')"
             class="inline-flex justify-center items-center py-3 px-5 text-base font-medium text-center text-white rounded-lg bg-red-600 hover:bg-red-800 focus:ring-4 focus:ring-blue-300 cursor-pointer">
            Cek tingkat kemacetan
            <svg class="w-3.5 h-3.5 ms-2 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                 fill="none" viewBox="0 0 14 10">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M1 5h12m0 0L9 1m4 4L9 9"/>
            </svg>
          </a>
          <button @click="openAboutModal"
                  class="py-3 px-5 sm:ms-4 text-sm font-medium text-white focus:outline-none bg-transparent rounded-lg border border-white hover:bg-white hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-white cursor-pointer">
            Tentang kami
          </button>
        </div>
      </div>
    </section>

    <!-- Modal Tentang Kami -->
    <transition name="modal-fade">
      <div v-if="isAboutModalOpen" class="fixed inset-0 z-50 overflow-y-auto">
        <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
          <!-- Overlay -->
          <div class="fixed inset-0 transition-opacity" @click="closeAboutModal">
            <div class="absolute inset-0 bg-black opacity-70"></div>
          </div>

          <!-- Modal Content -->
          <div class="inline-block align-bottom bg-gray-800 rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
               data-aos="zoom-in">
            <div class="bg-gray-800 px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
              <div class="sm:flex sm:items-start">
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                  <div class="flex justify-between items-center">
                    <h3 class="text-2xl leading-6 font-bold text-white" id="modal-title">
                      Tentang Kami
                    </h3>
                    <button @click="closeAboutModal" class="text-gray-400 hover:text-white">
                      <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
                      </svg>
                    </button>
                  </div>
                  <div class="mt-4">
                    <p class="text-gray-300">
                      Arial Project adalah inisiatif mahasiswa Teknik Elektro untuk mengatasi kemacetan di Bali melalui:
                    </p>
                    <ul class="mt-3 text-gray-300 list-disc list-inside space-y-2">
                      <li>Sistem lampu lalu lintas adaptif berbasis AI</li>
                      <li>Ditenagai oleh pembangkit listrik tenaga surya</li>
                      <li>Monitoring real-time kepadatan lalu lintas</li>
                    </ul>

                    <div class="mt-6 border-t border-gray-700 pt-4">
                      <h4 class="font-semibold text-white">Tim Pengembang:</h4>
                      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-3">
                        <div class="flex items-center space-x-3">
                          <div class="flex-shrink-0 h-10 w-10 rounded-full bg-blue-500 flex items-center justify-center text-white">
                            74
                          </div>
                          <div>
                            <p class="text-sm font-medium text-white">I Putu Gede Pringga A.P (Leader)</p>
                            <p class="text-xs text-gray-400">Mechanics & Power System</p>
                          </div>
                        </div>
                        <div class="flex items-center space-x-3">
                          <div class="flex-shrink-0 h-10 w-10 rounded-full bg-purple-500 flex items-center justify-center text-white">
                            63
                          </div>
                          <div>
                            <p class="text-sm font-medium text-white">Rayhan Kimi Nabiel A.</p>
                            <p class="text-xs text-gray-400">Backend & Computer Vision Devlp.</p>
                          </div>
                        </div>
                        <div class="flex items-center space-x-3">
                          <div class="flex-shrink-0 h-10 w-10 rounded-full bg-purple-500 flex items-center justify-center text-white">
                            58
                          </div>
                          <div>
                            <p class="text-sm font-medium text-white">Jonathan Marthen Mamboran</p>
                            <p class="text-xs text-gray-400">Electronics & Hardware Devlp.</p>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="bg-gray-700 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
              <button type="button"
                      @click="closeAboutModal"
                      class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-red-600 text-base font-medium text-white hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 sm:ml-3 sm:w-auto sm:text-sm">
                Tutup
              </button>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <!-- Rest of your existing template remains the same -->
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
      <div data-aos="fade-left" id="traffic-data" data-aos-duration="1000"
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
          <div  data-aos="fade-up" data-aos-delay="400" class="mb-8 pb-6 border-b border-gray-200">
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
/* Existing styles... */

/* Modal Transition */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

/* Modal Content Animation */
[data-aos="zoom-in"] {
  transform: scale(0.95);
  opacity: 0;
  transition-property: transform, opacity;
}

[data-aos="zoom-in"].aos-animate {
  transform: scale(1);
  opacity: 1;
}
</style>