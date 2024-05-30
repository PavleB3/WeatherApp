<template>
    <div id="wrapper1">
        <h1 id="header1">Today</h1>
        <p id="date">{{ date }}</p>
        <p id="time">{{ time }}</p>
        <h1 id="cityName">{{ name }}, {{ country }}</h1>
        <div class="tempDesc">
            <h1 id="temperature">{{ temperature }}&deg;C</h1>
            <h2 id="description">{{ description }}</h2>
        </div>
    </div>
    <div id="wrapper2">
        <div id="seaLevelWrapper">
            <div class="seaLevelColumn">
                <div class="seaLevelHeader">Sea Level</div>
                <div class="seaLevelData">{{ seaLevel }}</div>
            </div>
            <div class="seaLevelColumn">
                <div class="seaLevelHeader">Humidity</div>
                <div class="seaLevelData">{{ humidity }}</div>
            </div>
            <div class="seaLevelColumn">
                <div class="seaLevelHeader">Wind</div>
                <div class="seaLevelData">{{ wind }}</div>
            </div>
        </div>
        <daysWeather :cityName="cityName"/>
    </div>
</template>

<script>
import axios from "axios"
import daysWeather from "./DaysWeather.vue"
    export default{
        components:{
            daysWeather,
        },
        props:{
            city: String,
        },
        data(){
            return{
                cityName: this.city,
                temperature: null,
                description: null,
                iconUrl: null,
                date: null,
                time: null,
                name:null,
                monthNames: ["January", "February", "March", "April", "May", "June", "July",
                    "August", "September", "October", "November", "December"],
                seaLevel: null,
                wind: null,
                country: null,
                humidity: null,

            }
        },
        async created(){
            const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8331cdef4f10633c84fd856ce65588b0`);
            const weatherData = response.data;
            this.temperature = Math.round(weatherData.main.temp);
            this.description = weatherData.weather[0].description;
            this.name = weatherData.name; 
            this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon.png}`;
            const d = new Date();
            this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
            this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
            this.wind = weatherData.wind.speed;
            this.seaLevel = weatherData.main.feels_like;
            this.country = weatherData.sys.country; 
            this.humidity = weatherData.main.humidity;
            console.log(weatherData)
        }
    }
</script>

<style lang="scss" scoped>
    #wrapper1 {
        position:relative;
        margin-left: 252px;
        width: 700px;
        background: rgba(240, 250, 255, 0.8);
        margin-top: 5px;
        border-radius: 10px;
        height: 500px;
        color: black;
        position:relative;
        color:white;
        transition: transform 0.3s ease-in-out;
        background-image: url('/public/1_GsImz-edoeuqCMfKxDus0w.jpg')
    }
    #wrapper1:hover{
        transform:scale(1.05);
        z-index: 2;
    }
    #header1 {
        margin-left: 10px;
    }
    #date {
        font-size: larger;
        font-weight: 600;
        @extend #header1;
    }
    #time {
        font-size: larger;
        font-weight: 600;
        @extend #header1;
    }
    #wrapper2 {
        @extend #wrapper1;
        margin-left: 962px;
        margin-top: -500px;
        width: 690px;
        background:#212730;
        position:relative;
    }
    #seaLevelWrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        color:white;
    }
    .seaLevelColumn {
        display: flex;
        width: 100%;
        justify-content: space-between;
        margin-top:15px;
        margin-left:10px;
    }
    .seaLevelData {
        width: 10%;
        text-align: left;
        font-weight:lighter;
    }
    .seaLevelHeader{
        font-weight:bold;
    }
    .tempDesc{
        position:absolute;
        bottom:10px;
        left:5px;
    }
    #temperature{
        font-size:50px;
    }
    #cityName{
        @extend #header1;
    }
</style>
