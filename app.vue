<template>
  <Head>
    <Html lang="en" />
    <Title>{{ title }}</Title>
    <Meta name="description" :content="title" />
  </Head>
  <div class="p-5">
    <h1 class="mb-3 text-center text-sky-400 text-2xl md:text-3xl font-bold">Weather App</h1>
    <form class="flex mx-auto max-w-md">
      <input type="search" class="block p-2.5 ps-4 w-full z-20 text-sm text-gray-800 bg-gray-200 rounded-s-md outline-none border-0" placeholder="Search by city name" required autocomplete="off" v-model="input" />
      <button type="submit" class="flex items-center bg-sky-400 p-2.5 h-full text-sm font-medium text-white bg-blue-dark rounded-e-md outline-none">
        <svg class="mr-2 w-4 h-4" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
        </svg>
        search
      </button>
    </form>
    <div v-if="weatherDatas" class="mt-5 md:mt-5">
      <div>
        <img class="h-36 mx-auto" v-for="weatherData in weatherDatas.weather" :key="weatherData" :src="`/svg/${weatherData?.main}.svg`" alt="image weather" loading="lazy" />
        <div class="my-2">
          <p class="mt-4 text-center font-bold tracking-wider md:text-lg" v-for="weatherData in weatherDatas.weather" :key="weatherData">{{ weatherData.description }}</p>
          <p class="my-2 text-center fond-bold text-5xl">{{ Math.floor(weatherDatas.main.feels_like) }}°C</p>
          <div class="flex items-center justify-center md:mb-5">
            <img class="h-5 mr-1" src="./public/svg/location.svg" alt="location image" />
            <p class="text-lg font-semibold">
              {{ weatherDatas.name }}, <span>{{ weatherDatas.sys.country }}</span>
            </p>
          </div>
        </div>
      </div>
      <div class="grid text-center grid-cols-3 gap-4 mt-3 md:mx-auto md:row-span-2 md:max-w-md">
        <div class="grid grid-rows-2 items-center justify-center">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/temperature-down.svg" alt="temp min image" />
          <p class="fond-bold">{{ Math.floor(weatherDatas.main.temp_min) }}°C</p>
        </div>
        <div class="grid grid-rows-2 items-center justify-center border-x-2">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/temperature-up.svg" alt="temp max image" />
          <p class="fond-bold">{{ Math.floor(weatherDatas.main.temp_max) }}°C</p>
        </div>
        <div class="grid grid-rows-2 items-center justify-center">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/wind.svg" alt="wind image" />
          <p class="fond-bold">{{ Math.floor(weatherDatas.wind.speed) }}m/s</p>
        </div>
        <div class="grid grid-rows-2 items-center justify-center">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/pressure-2.svg" alt="pressure image" />
          <p class="fond-bold">{{ weatherDatas.main.pressure }}hPa</p>
        </div>
        <div class="grid grid-rows-2 items-center justify-center border-x-2">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/humidity.svg" alt="humidity image" />
          <p class="fond-bold">{{ weatherDatas.main.humidity }}%</p>
        </div>
        <div class="grid grid-rows-2 items-center justify-center">
          <img class="block mx-auto h-5 md:h-6" src="./public/svg/visibility.svg" alt="visibility image" />
          <p class="fond-bold">{{ (weatherDatas.visibility / 1000).toFixed(1) }}km</p>
        </div>
      </div>
    </div>
    <div v-else-if="pending">
      <loading />
    </div>
    <div v-else-if="error && input.length !== 0">
      <Error />
    </div>
  </div>
</template>
<script setup>
  const id = useRuntimeConfig().public.apiKey;
  const title = 'Nuxt Weather';
  const input = ref('');
  const {
    data: weatherDatas,
    pending,
    error,
  } = await useLazyFetch(() => {
    return `https://api.openweathermap.org/data/2.5/weather?q=${input.value}&units=metric&appid=${id}`;
  });
  // console.log(weatherDatas);
</script>

<style scoped>
  body {
    background-color: #f0f0f0;
  }
</style>
