<script>
import axios from 'axios'
export default {
    data() {
        return {
            city: '',
            error: '',
            info: null
        }
    },
    computed: {
        cityName() {
            return '«'+ this.city +'»'
        },
        showTemp() {
            return 'Температура ' + this.info.data.main.temp
        },
        feelsTemp() {
            return 'Ощущается как  ' + this.info.data.main.feels_like
        },
        minTemp() {
            return 'Минимальная температура  ' + this.info.data.main.temp_min
        },
        maxTemp() {
            return 'Максимальная температура  ' + this.info.data.main.temp_max
        }
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2) {
                this.error = 'Нужно название более одного символа :)'
                return false
            }
            this.error = ''
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3e16d93b47613ddaf9368ac74719a744`)
            .then(res => (this.info = res))
        }
    }
}
</script>

<template>
    <div class="wrapper">
        <div class="greetings">
            <h1 class="greetings__title">Погодное приложение</h1>
            <div class="greetings__subtitle">Узнайте погоду в {{ city == '' ? ' вашем городе' : cityName }}</div>
        </div>
        <div class="choosingcity">
            <input type="text" v-model="city" placeholder="Введите город" @keyup.enter="getWeather()">
            <transition name="slide-fade" :duration="10">
                <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
            </transition>
            <div class="choosingcity__error">
                {{ error }}
            </div>
        </div>
        <div class="weatherinfo" v-if="info">
            <div class="weatherinfo__block">{{ showTemp }}</div>
            <div class="weatherinfo__block">{{ feelsTemp }}</div>
            <div class="weatherinfo__block">{{ minTemp }}</div>
            <div class="weatherinfo__block">{{ maxTemp }}</div>
        </div>  
    </div>
</template>

<style scoped>
    .wrapper {
    padding: 20px;
    width: 900px;
    height: 500px;
    display: flex;
    flex-direction: column;
    border-radius: 50px;
    background-color: #cbcbca;
    text-align: center;
}
    .greetings__title {
        margin-top: 10px;
    }
    .greetings__subtitle {
        margin-top: 10px;
        letter-spacing:0.1rem;
    }
    .choosingcity input {
        margin-top: 30px;
        width: 300px;
        background-color: transparent;
        border: 0;
        border-bottom: 2px solid #2d2a2e;
        font-size: 1rem;
        padding: 5px 8px;
        outline: none;
    }
    .choosingcity input:focus {
        border-bottom-color: #e3bc4b;
    }
    .choosingcity button {
        background: #e3bc4b;
        color: #fff;
        border-radius: 10px;
        border: 2px solid #b99935;
        padding: 10px 15px;
        margin-left: 20px;
        cursor: pointer;
        transition: transform 500ms ease;
    }
    .weatherinfo__block {
        margin-top: 10px;
    }
    .choosingcity button:hover {
        transform: scale(1) translateY(-5px);
    }
    .choosingcity__error {
        margin-top: 5px;
        color: #c03d3d;
    }
    .slide-fade-enter-active {
    transition: all 0.3s ease-out;
    }

    .slide-fade-leave-active {
    transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
    }

    .slide-fade-enter-from,
    .slide-fade-leave-to {
    transform: translateX(20px);
    opacity: 0;
    }
</style>
