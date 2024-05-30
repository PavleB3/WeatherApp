<template>
    <div class="cardsWrapper">
        <div class="weatherBox" v-for="day in forecast" :key="day.date"><span>{{getDayName(day.date)}}</span>
            <p id="temperature1">{{ day.temperature }}&deg;C</p>
            <i class="fas fa-cloud" id="cloud"></i>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
    export default{
        props:{
            cityName: String
        },
        data(){
            return{
                forecast: [],
 
            }
        },
        mounted(){
            this.fetchWeatherData();
        },
        methods:{
            async fetchWeatherData(){
                const apiKey = '8331cdef4f10633c84fd856ce65588b0';
                const city = this.cityName;
                const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`

                await axios.get(apiUrl).then(response => {
                    const forecastData = response.data.list;
                    const filteredData = forecastData.map(item =>{
                        return {
                            date: moment(item.dt_txt.split(' ')[0]),
                            temperature: Math.round(item.main.temp),
                            description: item.weather[0].description,
                        }
                    }).reduce((acc,item) =>{
                        if(!acc.some(day => day.date.isSame(item.date, 'day'))){
                            acc.push(item);
                        }
                        return acc;
                    }, []).slice(1,4);
                    console.log(filteredData, "working");
                    this.forecast = filteredData;
                }).catch(error => {
                    console.error('Error fetching weather data: ', error)
                    this.loading = false;
                })
                
            },
            getDayName(date){
                return date.format('ddd');
            }
        }
    }
</script>

<style>
    .weather-card{
        width:170px;
        height:250px;
        margin-top:40px;
        margin-left:20px;
        margin-right:20px;
        border-radius:10px;
        background:rgba(10, 68, 129, 1);
        color:white;
        transition: transform 0.3s ease-in-out;
    }
    .weather-card:hover{
        cursor:default;
        transform: scale(1.1);
        
    }
    .cardsWrapper{
        position:relative;
        top:30px;
        height:250px;
        width:100%;
        background:#212730;
        border-radius:10px;
        margin-top:30px;
        margin-bottom:50px;
        box-shadow: 4px 4px 8px rgba(0, 0, 0, 0.5);
        border: 1px solid rgba(0, 0, 0, 0.2);
        overflow: hidden;
        display:flex;
        flex-direction:row;
        justify-content: space-between;
        align-items: center;
        
    }
    .button{
        width:100%;
        display:flex;
        justify-content: center;
        margin-top:10px;
    }
    #locationButton{
        background:rgba(52, 110, 203, 1);
        color:white;
    }
    .weatherBox{
        height:200px;
        width:150px;
        margin-left:20px;
        margin-right:20px;
        border-radius:10px;
        transition: transform 0.3s ease-in-out;
        position:relative;
        text-align: center;
        display:flex;
        justify-content: center;
        background-image: url('/public/Screenshot_2.png');
    }
    .weatherBox:hover{
        transform:scale(1.07);
    }
    #temperature1{
        position:absolute;
        top:140px;
        left:57px;
    }
    span{
        position:absolute;
        top:60px;
        font-size:20px;
    }
    #cloud{
        position:absolute;
        top:10px;
        font-size:27px;
    }

</style>