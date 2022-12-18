<script>
import axios from "axios";

export default {
    data() {
        return {
            city: "",
            error: "",
            info: null
        }
    },
    computed: {
        cityName() {
            return this.city === "" ? "вашем городе" : "«" + this.city + "»"
        },
        showTemperature() { return 'Температура: ' + this.info.main.temp + '°C' },
        showFeelsLike() { return 'Ощущается: ' + this.info.main.feels_like + '°C' },
        showPressure() { return 'Давление: ' + this.info.main.pressure + 'мм рт ст' },
        showHumidity() { return 'Влажность: ' + this.info.main.humidity + '%' },
        showWindSpeed() { return 'Скорость ветра: ' + this.info.wind.speed + 'м/с' }
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2) {
                this.error = "Введите больше одного символа"
                return false
            }
            this.error = ""

            const APIkey = 'b298aad0de6a395bf52fd7d677e45168';
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${APIkey}`)
                .then(res => this.info = res.data)
        },
    }
}
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ cityName }}</p>
        <input type="text" v-model="city" placeholder="Введите город" />
        <button disabled v-if="city === ''">Введите название города</button>
        <button v-else @click="getWeather()">Узнать погоду</button>
        <p class="error">{{ error }}</p>

        <div  class="info" v-if="info !== null">
            <p>{{ showTemperature }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ showPressure }}</p>
            <p>{{ showHumidity }}</p>
            <p>{{ showWindSpeed }}</p>
        </div>
    </div>
</template>

<style scoped>
.info {
    color: #faffb0;
}

.error {
    color: #d03939;
}

.div {
    display: flex;
    align-items: center;
    justify-content: center;
}

.wrapper {
    width: 600px;
    height: 400px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: #fff;
}

.wrapper h1 {
    margin-top: 50px;
}

.wrapper p {
    margin-top: 20px;
}

.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
    background: #746027;
    cursor: not-allowed;
}

.wrapper button {
    background: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}
</style>
