<template class="bg-[#f0f0f0]">
  <Head>
    <Title>{{ title }}</Title>
    <Meta name="description" :content="title" />
  </Head>
  <div class="px-3 pt-6 pb-10">
    <!-- <h1 class="my-3 text-xl text-center font-semibold md:text-2xl">Weather</h1> -->
    <form class="flex mx-auto max-w-md">
      <input class="flex-1 bg-gray-300 rounded-l-xl border-0 outline-0 px-4 py-2.5 md:px-6" v-model="input" placeholder="Search with city name" type="text" />
      <button class="block px-3 py-1.5 rounded-r-xl text-white bg-cyan-500 md:px-6" type="button">Search</button>
    </form>
    <div v-if="weatherDatas" class="mt-5 md:mt-5 md:flex md:flex-col md:justify-center md:gap-x-24 md:w-full">
      <div>
        <img class="h-36 mx-auto" v-for="weatherData in weatherDatas.weather" :key="weatherData" :src="`/svg/${weatherData?.main}.svg`" alt="image weather" />
        <div class="my-2">
          <p class="mt-4 text-center font-bold tracking-wider md:text-lg" v-for="weatherData in weatherDatas.weather" :key="weatherData">{{ weatherData.description }}</p>
          <p class="my-2 text-center fond-bold text-5xl">{{ Math.floor(weatherDatas.main.feels_like) }}°C</p>
        </div>
      </div>
      <div class="md:flex md:flex-col md:h-full md:items-center md:text-lg">
        <div class="flex items-center justify-center md:mb-5">
          <img class="h-5 mr-1" src="./public/svg/location.svg" alt="location image" />
          <p class="text-lg font-semibold">
            {{ weatherDatas.name }}, <span>{{ weatherDatas.sys.country }}</span>
          </p>
        </div>
        <div class="grid text-center grid-cols-3 gap-4 mt-3 md:m-0 md:row-span-2">
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
    </div>
    <div v-else-if="pending" class="max-w-md mx-auto -mt-2 px-4 py-2.5 text-center text-white bg-cyan-400">loading bang</div>
    <div v-else-if="error && input.length !== 0" class="max-w-md mx-auto -mt-2 px-4 py-2.5 text-center text-white bg-red-400">nggak ketemu</div>
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
