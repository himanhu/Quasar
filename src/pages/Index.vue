<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input 
      bottom-slots 
      v-model="search" 
      placeholder="Search" 
      borderless
      >
        <template v-slot:before>
          <q-icon 
          @click="getLocation" 
          name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>
  <template v-if="weatherData">
    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">
          {{weatherData.name}}
      </div>
      <div class="text-h6 text-weight-light">
        {{weatherData.weather[0].main}}
      </div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>{{Math.round(weatherData.main.temp)}}</span>
        <span class="text-h4 relative-position degree"> &deg;</span>
      </div>
    </div>

    <div class="col text-center">
      <img src="https://www.fillmurray.com/100/100" alt="">
    </div>
    
  </template>
  <template v-else>
    <div class="col column text-center text-white">
      <div class="col text-h2 text-weight-thin">
        Quasar<br>Weather
      </div>
    <q-btn class="col" 
    @click="getLocation"
    flat>
      <q-icon left size="3em"  name="my_location" />
      <div>Find my Location</div>
    </q-btn>
    </div>
  </template>
    <div class="skyline">
      <div class="col skyline">

      </div>

    </div>
  </q-page>  
</template>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData : null,
      lat: null,
      lon:null,
      apiUrl:'https://api.openweathermap.org/data/2.5/weather',
      apiKey:'c5a45aca660147b743b4aea21c3cc8b3'
    }
  },
  methods:{
    getLocation(){
      navigator.geolocation.getCurrentPosition(position=>{
        console.log('position',position)
        this.lat =position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords(){
      this.$axios(`${this.apiUrl }?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}& units=metric`).then(Response=>{
        this.weatherData=Response.data
      })
    }
  }

}
</script>
<style lang="sass">
 .q-page
   background: url(../statics/skyline1.jpg)
   .degree
     top: -44px
   
</style>
