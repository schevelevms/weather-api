<template>
  <div class="weather" :class="weatherSeacrh">
    <div class="container">
        <header class="weather-header">
            <div class="weather-header__logo">weather.com</div>
        <main class="weather-main">
            <div class="weather-main-inputform">
            <input type="text" @keyup.enter="submit" v-model="searchQueary" class="weather-main__input" placeholder="Enter country">
            <button class="weather-main__btn" @click="submit">Search</button>
        </div>
        </main>
        </header>
        
        <main class="main">
            <div class="weather-main block" v-if="!error && location && temperature && description">
                <div class="weather-loader" v-if="loading">Loading...</div>
            <template v-else>
                <h1 class="weather__city">{{location}}</h1>
                <div class="weather-main-info">
                    <div class="weather-main-top">
                        <div class="weather-main-info__temp">{{temperature}}<span style="font-size: 40px; align-items: start;">°c</span></div>
                        <img v-if="image" :src='image' class="weather-main-info__icon" alt="weather">
                    </div>
                        <div class="weather-main-info__name">{{description}}</div>
                </div>
            </template>
            </div>
            <div class="weather-side" v-if="!loading && location && !error">
                <div class="weather-side-block">
                    <div class="weather-side-block-info">
                    <h2>Wind</h2>
                    <img class="weather-side-block-info-img" src="../assets/icons-params/wind.svg" alt="wind">
                    </div>
                    <div class="param">{{ wind }} km/h</div>
                </div>
                <div class="weather-side-block">
                    <div class="weather-side-block-info">
                    <h2>Humidity</h2>
                    <img class="weather-side-block-info-img" src="../assets/icons-params/humidity.svg" alt="humidity">    
                    </div>
                    <div class="param">{{ humidity }}%</div>   
                </div>
                <div class="weather-side-block">
                    <div class="weather-side-block-info">
                    <h2>Visibility</h2>
                    <img class="weather-side-block-info-img" src="../assets/icons-params/vis.svg" alt="vis">    
                    </div>
                    <div class="param">{{ vis }} km</div>
                </div>
                <div class="weather-side-block">
                    <div class="weather-side-block-info">
                    <h2>Feels Like</h2>
                    <img class="weather-side-block-info-img" src="../assets/icons-params/feels.svg" alt="feels">
                    </div>
                    <div class="param">{{ feels }}<span style="font-size: 17px; align-items: start;">°c</span></div>
                </div>
            </div>
         </main>   
        <div class="error" v-if="error">This is not a not country/city</div>
        
    </div>
    <div class="weather-bg">
        <div class="weather-bg__shell">
            <img class="weather-bg__img bg" src="../assets/background/main.jpg" alt="main">
            <img class="weather-bg__img sunny" src="../assets/background/sunny.jpg" alt="sunny">
            <img class="weather-bg__img snows" src="../assets/background/snow.webp" alt="snows">
            <img class="weather-bg__img clear" src="../assets/background/clear.jpg" alt="clear">
            <img class="weather-bg__img cloudy" src="../assets/background/cloudy.jpg" alt="cloudy">
            <img class="weather-bg__img rain" src="../assets/background/rain.jpg" alt="rain"> 
            <img class="weather-bg__img mist" src="../assets/background/mist.jpg" alt="mist"> 
        </div>
    </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            location: '',
            temperature: 0,
            description: '',
            feels: 0,
            vis: 0,
            humidity: 0,
            wind: 0,
            loading: false,
            searchQueary: '',
            error: false
            
        }
    },
    computed:{
        weatherSeacrh(){
            if (this.description.includes('snow') ||  this.description.includes('Overcast')) {
                return 'snows'
            }else if(this.description.includes('Sunny')){
                return 'sunny'
            }else if(this.description.includes('Clear')){
                return 'clear'
            }else if(this.description.includes('cloudy') || this.description.includes('Cloudy')){
                return 'cloudy'
            }else if(this.description.includes('rain') || this.description.includes('Rain')){
                return 'rain'
            }else if(this.description.includes('Mist') || this.description.includes('Fog')){
                return 'mist'
            }
        },
        image(){
            if(this.description.includes('snow') ||  this.description.includes('Overcast')) {
                return '/src/assets/icons-main/snows.svg'
            }else if(this.description.includes('Sunny')){
                return '/src/assets/icons-main/sunny.svg'
            }else if(this.description.includes('Clear')){
                return '/src/assets/icons-main/clear.svg'
            }else if(this.description.includes('cloudy') || this.description.includes('Cloudy')){
                return '/src/assets/icons-main/cloudy.svg'
            }else if(this.description.includes('rain') || this.description.includes('Rain')){
                return '/src/assets/icons-main/rain.svg'
            }else if(this.description.includes('mist') || this.description.includes('Mist')){
                return '/src/assets/icons-main/mist.svg'
            }
        }
    },
    methods:{
        submit(){
            this.loading = true
            fetch(`https://api.weatherapi.com/v1/current.json?key=d021e9e0dec848d1b49173753260301&q=
            ${this.searchQueary}`)
            .then(response => response.json())
            .then(data => {
                this.loading = false
                this.error = false
                this.location = data.location.name
                this.temperature = data.current.temp_c
                this.description = data.current.condition.text
                this.vis = data.current.vis_km
                this.humidity = data.current.humidity
                this.wind = data.current.wind_mph
                this.feels = data.current.feelslike_c
                this.searchQueary = '' 
                       
            })
            .catch(e => {
                console.log(e);
                this.searchQueary = ''
                this.loading = false
                this.error = true
            })
        }
    }
}
</script>

<style>

</style>