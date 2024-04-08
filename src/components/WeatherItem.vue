<template>

    <div :class="fullSize ? 'full_post' : 'post'"  :style="{backgroundImage:`url(${this.url})`}">
    <table>
        <tr><td><strong>{{ item.city }}</strong><br><p v-if="info != ''" :class="fullSize ? 'big' : ''"><strong>{{ cityDate.hour }}:{{ cityDate.min }}</strong></p></td>

        <td><div v-if="info != ''" :class="fullSize ? 'big' : ''"><strong>{{ showTemp }}&deg;C </strong> </div>
        <div v-if="info != ''"><strong>{{ weather }} </strong></div></td>

        <td><show-button
        @click="fullSize = !fullSize;"
        ></show-button>
        <delete-button
        @click="$emit('remove', item)"
        ></delete-button></td></tr>

        <tr><td>
            <div v-if="fullSize"> 
            <div v-if="info != ''">{{ cityDate.date }}-{{ cityDate.month }}-{{ cityDate.year }}<br>
            <strong>{{ cityDate.day }}</strong><br>
            {{SunRise}} / {{SunSet}}</div>
            </div></td><td>
            

     <div v-if="fullSize"> 
        <div v-if="info != ''"><strong>Влажность:</strong> {{ showHumidity }} </div>
    </div>

    <div v-if="fullSize"> 
        <div v-if="info != ''"><strong>Ветер:</strong> {{ showWind }} </div>
    </div>

    <div v-if="fullSize"> 
        <div v-if="info != ''"><strong>Видимость:</strong> {{ this.info.visibility }}м </div>
    </div></td></tr>
        
        </table>
</div>
    
</template>

<script>


export default { 
    props: {
        item: {
            type: Object, 
            required: true
        }
    },
    data () {
    return {
      city: this.item.city,
      info: '',
      Fulltime: '',
      fullSize: false,
      icon: '01d',
      url: require(`@/img/01d.jpg`)
    }
    },
    methods: {
        getWeather() {
         fetch (`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=aa65dec7f4fa17a5b530693cc10007a0&units=metric`)
        .then(response => response.json())
        .then(response => (this.info = response))
        .then(response => (this.url = require('@/img/' + response.weather[0].icon + '.jpg')));
        } 
    },
     mounted() {
        this.getWeather();
    },
    computed: { 
        cityDate() {
            let date = new Date();
            let localTime = date.getTime();
            let localOffset = date.getTimezoneOffset() * 60000;
            let utc = localTime + localOffset;
            let cityTime = utc + (1000 * this.info.timezone);
            let cityDate = new Date(cityTime);
            const months = ['Января','Февраля','Марта','Апреля','Мая','Июня','Июля','Августа','Сентября','Октября','Ноября','Декабря'];
            const days = ['Воскресенье', 'Понедельник', 'Вторник', 'Среда', 'Четверг', 'Пятница', 'Суббота'];
            const year = cityDate.getFullYear();
            const month = months[cityDate.getMonth()];
            const num = cityDate.getDate();
            const day = days[cityDate.getDay()];
            const hour = cityDate.getHours();
            const min = cityDate.getMinutes();
            const time = {date: num, month: month, year: year, day: day, hour: ('00' + hour).slice(-2),  min: ('00' + min).slice(-2)};
            return time;
        },
        showTemp() {
            return Math.round(this.info.main.temp) + " "
        },
        showHumidity() {
            return this.info.main.humidity + "%"
        },
        showWind() {
            let WindDeg;
            if (this.info.wind.deg <= 10 && this.info.wind.deg >= 350) WindDeg = 'C, ';
            else if (this.info.wind.deg <= 100 && this.info.wind.deg >= 80) WindDeg = 'В, ';
            else if (this.info.wind.deg <= 190 && this.info.wind.deg >= 170) WindDeg = 'Ю, ';
            else if (this.info.wind.deg <= 280 && this.info.wind.deg >= 260) WindDeg = 'З, ';
            else if (this.info.wind.deg < 80 && this.info.wind.deg > 10) WindDeg = 'СВ, ';
            else if (this.info.wind.deg < 350 && this.info.wind.deg > 280) WindDeg = 'СЗ, ';
            else if (this.info.wind.deg < 260 && this.info.wind.deg > 190) WindDeg = 'ЮЗ, ';
            else if (this.info.wind.deg < 170 && this.info.wind.deg > 100) WindDeg = 'ЮВ, ';
            else WindDeg = '';
            return WindDeg + this.info.wind.speed + " м/с"
        },
        weather() {
            const weather = this.info.weather[0].main;
            switch (weather) {
                case 'Thunderstorm':
                    return 'Гроза';
                case 'Drizzle':
                    return 'Небольшой дождь';
                case 'Rain':
                    return 'Дождь';
                case 'Snow':
                    return 'Снег';
                case 'Mist':
                    return 'Туман';
                case 'Smoke':
                    return 'Дым';
                case 'Haze':
                    return 'Туман';
                case 'Dust':
                    return 'Пыль';
                case 'Fog':
                    return 'Туман';
                case 'Sand':
                    return 'Песчаный вихрь';
                case 'Ash':
                    return 'Пепел';
                case 'Squall':
                    return 'Ураган';
                case 'Tornado':
                    return 'Торнадо';
                case 'Clear':
                    return 'Ясно';
                case 'Clouds':
                    return 'Облачно';
                default:
                    return weather;
            }
            
        },
        SunRise() {
            const a = new Date(this.info.sys.sunrise * 1000);
            const hour = a.getHours();
            const min = a.getMinutes();
            const time = ('00' + hour).slice(-2) + ':' + ('00' + min).slice(-2);
            return "Восход: " +  time + " ";
        },
        SunSet() {
            const a = new Date(this.info.sys.sunset * 1000);
            const hour = a.getHours();
            const min = a.getMinutes();
            const time = ('00' + hour).slice(-2) + ':' + ('00' + min).slice(-2);
            return " Закат: " +  time;
        },
    }
}

</script>

<style scoped>
.post {
    padding: 15px;
    color: white ;
    font-family: Arial;
    font-size: 3 rem;
    border-radius: 20px;
    margin-top: 15px;
    display: flex;
}

table {
    width: 100%;   
}

td {
    width: 10%;
}
td:nth-child(2) {
    text-align: center;
}

td:nth-child(3) {
    text-align: right;
}

.full_post {
    padding: 15px;
    color: white ;
    font-family: Arial;
    font-size: 1.2rem;
    border-radius: 20px;
    margin-top: 15px;
    min-height: 200px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.big {
    font-size: 3rem;
}

</style>