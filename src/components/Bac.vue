<template>
    <div class="container pt-1">
        <h1 class="my-3 bold">Scattergories</h1>
        <div v-if="!isTimerSet">
            <h3 class="my-5">Challenge yourself with a custom timer !<br/>The less time you have, the more points you'll receive !</h3>
            <h3>Timer of {{ minuteChose }} minutes</h3>
            <div class="row justify-content-sm-center mt-3 mb-5">
                <input type="range" class="col-lg-6 col-sm-12 custom-range" min="1" max="10" id="input-timer-value" @input="updateTimer()">
            </div>
            <button @click.prevent="setTimer()" class="btn btn-lg btn-success">Play !</button>
        </div>
        <div class="row justify-content-sm-center" v-else>
            <div v-if="!timerOver" class="mb-5 col-lg-3 col-sm-12 alert alert-secondary">
                Time left :<br>
                <h4 :class="timer <= 10 ? 'red bold' : 'bold'">{{formatTwoDigit(Math.floor(timer / 60))}}:{{formatTwoDigit((timer % 60))}}</h4>
            </div>
            <div v-else class="mb-5 col-3 alert alert-secondary">Time left :<br><h4 :class="timer <= 10 ? 'red bold' : 'bold'">{{formatTwoDigit(Math.floor(timeLeft / 60))}}:{{formatTwoDigit((timeLeft % 60))}}</h4></div>
            
            <div class="table-responsive">
                <table class="table table-bordered mb-5 col-sm-12 table-condensed cf">
                    <thead>
                        <tr>
                        <th scope="col">Letter</th>
                        <th scope="col">Capital</th>
                        <th scope="col">Country</th>
                        <th scope="col">Fruit / Vegetable</th>
                        <th scope="col">Sport</th>
                        <th scope="col">Colour</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <th scope="row" class="bold">{{ letter }}</th>
                            <td :class="isWordCapital"><input :disabled="timerOver" id="input-capital" type="text" class="form-control" aria-label="Capital"></td>
                            <td :class="isWordCountry"><input :disabled="timerOver" id="input-country" type="text" class="form-control" aria-label="Country"></td>
                            <td :class="isWordFruit"><input :disabled="timerOver" id="input-fruit" type="text" class="form-control" aria-label="Fruit"></td>
                            <td :class="isWordSport"><input :disabled="timerOver" id="input-sport" type="text" class="form-control" aria-label="Sport"></td>
                            <td :class="isWordColor"><input :disabled="timerOver" id="input-color" type="text" class="form-control" aria-label="Color"></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <button v-if="!timerOver" class="btn btn-lg btn-primary col-lg-3 col-sm-8" @click.prevent="validateForm()">Submit !</button>
        </div>
        <div v-if="timerOver" class="row justify-content-sm-center">
            <div class="col-6 alert alert-warning">
                <h1>Time over !</h1>
                <h2 class="mt-3">Score : <span class="bold">{{score}}</span></h2>
                <div class="row justify-content-sm-center my-4">
                    <button class="col-4 btn btn-md btn-primary mx-3" @click.prevent="reset()">Play again !</button>
                    <button class="col-4 btn btn-md btn-secondary mx-3" @click.prevent="refresh()">Back to home !</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
const axios = require('axios')
axios.defaults.baseURL = 'https://api.dictionaryapi.dev/api/v2/entries/en_US';

export default{
    name: 'Bac',
    data() {
        return {
            letter: String,
            isWordCapital: '',
            isWordFruit: '',
            isWordSport: '',
            isWordCountry: '',
            isWordColor: '',
            minuteChose: 10,
            timer: Number,
            isTimerSet: false,
            timerOver: false,
            timeLeft: 0,
            score: 0,
        }
    },
    mounted (){
        var characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
        this.letter = characters.charAt(Math.floor(Math.random() * characters.length));
    },
    methods: {
        // https://api.dictionaryapi.dev/api/v2/entries/en_US/
        validateForm(){
            this.timeLeft = this.timer;
            this.isCapital();
            this.isSport();
            this.isColor();
            this.isCountry();
            this.isFruit();
            setTimeout(() => {
                console.log(this.score)
                this.timerOver = true;
                this.score = Math.round(this.score * (11 - this.minuteChose) * (10 * (this.timeLeft / (this.minuteChose*60))));
            },1000)
        },
        updateTimer(){
            this.minuteChose = document.getElementById("input-timer-value").value;
        },
        setTimer(){
            this.isTimerSet = true;
            this.minuteChose = document.getElementById("input-timer-value").value;
            this.timer = this.minuteChose * 60;
            setInterval(() => {
                if(this.timer > 0) this.timer--;
                else {
                    this.timerOver = true;
                }
            }, 1000);        
        },
        reset(){
            var characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            this.letter = characters.charAt(Math.floor(Math.random() * characters.length));
            this.isWordCapital = '';
            this.isWordFruit = '';
            this.isWordSport = '';
            this.isWordCountry = '';
            this.isWordColor = '';
            this.minuteChose = 10;
            this.timer = 999999999;
            this.isTimerSet = false;
            this.timerOver = false;
            this.timeLeft = 0;
            this.score = 0;
        },
        refresh(){
            location.reload();
        },
        formatTwoDigit(x){
            return x.toLocaleString('en-US', {
                minimumIntegerDigits: 2,
                useGrouping: false
            })
        },
        isCapital(){
            var word = document.getElementById('input-capital').value;
            // Capital
            if(word != "" && word.length > 3 && word[0].toUpperCase() == this.letter){
                axios.get('/' + word).then(res => {
                    if(JSON.stringify(res).toLowerCase().includes("capital")) {
                        this.isWordCapital = 'bg-success'
                        this.score++;
                    }
                    else this.isWordCapital = 'bg-danger'
                }).catch(err => {
                    console.error(err)
                    this.isWordCapital = 'bg-danger'
                });
            }
            else this.isWordCapital = 'bg-danger';
        },
        isCountry(){
            var word = document.getElementById('input-country').value;
            // country
            if(word != "" && word.length > 3 && word[0].toUpperCase() == this.letter){
                axios.get('/' + word).then(res => {
                    if(JSON.stringify(res).toLowerCase().includes("country")) {
                        this.isWordCountry = 'bg-success'
                        this.score++;
                    }
                    else this.isWordCountry = 'bg-danger'
                }).catch(err => {
                    console.error(err)
                    this.isWordCountry = 'bg-danger'
                });
            }
            else this.isWordCountry = 'bg-danger';
        },
        isFruit(){
            var word = document.getElementById('input-fruit').value;
            // fruit plant vegetable
            if(word != "" && word.length > 3 && word[0].toUpperCase() == this.letter){
                axios.get('/' + word).then(res => {
                    if(JSON.stringify(res).toLowerCase().includes("fruit")) {
                        this.isWordFruit = 'bg-success'
                        this.score++;
                    }
                    else this.isWordFruit = 'bg-danger'
                }).catch(err => {
                    console.error(err)
                    this.isWordFruit = 'bg-danger'
                });
            }
            else this.isWordFruit = 'bg-danger';
        },
        isSport(){
            var word = document.getElementById('input-sport').value;
            // Game Sport
            if(word != "" && word.length > 3 && word[0].toUpperCase() == this.letter){
                axios.get('/' + word).then(res => {
                    if(JSON.stringify(res).toLowerCase().includes("game")) {
                        this.isWordSport = 'bg-success'
                        this.score++;
                    }
                    else this.isWordSport = 'bg-danger'
                }).catch(err => {
                    console.error(err)
                    this.isWordSport = 'bg-danger'
                });
            }
            else this.isWordSport = 'bg-danger';
        },
        isColor(){
            var word = document.getElementById('input-color').value;
            // color colour
                if(word != "" && word.length > 3 && word[0].toUpperCase() == this.letter){
                axios.get('/' + word).then(res => {
                    if(JSON.stringify(res).toLowerCase().includes("color")) {
                        this.isWordColor = 'bg-success'
                        this.score++;
                    }
                    else this.isWordColor = 'bg-danger'
                }).catch(err => {
                    console.error(err)
                    this.isWordColor = 'bg-danger'
                });
            }
            else this.isWordColor = 'bg-danger';
        },
    }
}

</script>

<style scoped>
.white{
    color: white;
}
.bold{
    font-weight: 800;
}
.red{
    color: red;
}
</style>