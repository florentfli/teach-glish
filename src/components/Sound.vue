<template>
    <div class="container pt-1">
        <h1 class="mt-3 bold">List'n Guess</h1>
        <div v-if="!soundGo">
            <h3 class="my-5">Listen to the soundtracks and guess what you just hear !</h3>
            <button class="btn btn-lg btn-success" @click="launchSound()">Start !</button>
        </div>
        <div v-else-if="!gameOver">
            <div v-if="!choseTheme">
                <h1 class="my-5">Chose your theme :</h1>
                <div class="row justify-content-md-center">
                    <div class="mb-3 col-lg-2 col-sm-4">
                        <button class="btn-primary btn-lg h-100 w-100" @click="hasChoseTheme('animals')">Animals</button>
                    </div>
                    <div class="mb-3 col-lg-2 col-sm-4">
                        <button class="btn-primary btn-lg h-100 w-100" @click="hasChoseTheme('commons')">Every day life</button>
                    </div>
                    <div class="mb-3 col-lg-2 col-sm-4">
                        <button class="btn-primary btn-lg h-100 w-100" @click="hasChoseTheme('instruments')">Music instruments</button>
                    </div>
                </div>
            </div>
            <div v-else class="justify-content-sm-center row">
                <h3 class="col-lg-2 col-sm-6 alert alert-secondary my-5">Score : <span class="bold">{{ score }}/10</span></h3>
                <div id="col-12 audio-container">
                    <!-- ANIMALS -->
                    <audio v-if="sounds[theme][currentIndex] == 'cat'" controls="controls" preload="auto" src="/assets/sounds/cat.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'cicada'" controls="controls" preload="auto" src="/assets/sounds/cicada.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'cricket'" controls="controls" preload="auto" src="/assets/sounds/cricket.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'donkey'" controls="controls" preload="auto" src="/assets/sounds/donkey.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'dog'" controls="controls" preload="auto" src="/assetsd/soundsd/dog.mp3">
                        Your browser does not support the audio tag.
                    </audio>    
                    <audio v-if="sounds[theme][currentIndex] == 'duck'" controls="controls" preload="auto" src="/assetsd/soundsd/duck.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'elephant'" controls="controls" preload="auto" src="/assetsd/soundsd/elephant.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'goat'" controls="controls" preload="auto" src="/assetsd/soundsd/goat.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'sheep'" controls="controls" preload="auto" src="/assetsd/soundsd/sheep.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'wolf'" controls="controls" preload="auto" src="/assets/sounds/wolf.mp3">
                        Your browser does not support the audio tag.
                    </audio>

                    <!-- COMMONS -->
                    <audio v-if="sounds[theme][currentIndex] == 'car'" controls="controls" preload="auto" src="/assets/sounds/car.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'plane'" controls="controls" preload="auto" src="/assets/sounds/plane.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'doorbell'" controls="controls" preload="auto" src="/assets/sounds/doorbell.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'fireworks'" controls="controls" preload="auto" src="/assets/sounds/fireworks.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'helicopter'" controls="controls" preload="auto" src="/assets/sounds/helicopter.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'siren'" controls="controls" preload="auto" src="/assets/sounds/siren.mp3">
                        Your browser does not support the audio tag.
                    </audio>

                    <!-- INSTRUMENT -->
                    <audio v-if="sounds[theme][currentIndex] == 'cello'" controls="controls" preload="auto" src="/assets/sounds/cello.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'drums'" controls="controls" preload="auto" src="/assets/sounds/drums.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'guitar'" controls="controls" preload="auto" src="/assets/sounds/guitar.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'piano'" controls="controls" preload="auto" src="/assets/sounds/piano.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'trumpet'" controls="controls" preload="auto" src="/assets/sounds/trumpet.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                    <audio v-if="sounds[theme][currentIndex] == 'violin'" controls="controls" preload="auto" src="/assets/sounds/violin.mp3">
                        Your browser does not support the audio tag.
                    </audio>
                </div>
                
                <div class="row justify-content-md-center mt-4">
                    <div class="col-lg-4 col-sm-12">
                        <div class="input-group mb-3">
                            <span class="input-group-text" id="basic-addon1">Your guess</span>
                            <input :disabled="nbRound==3 || isCorrect" id="user-guess" type="text" pattern="[A-Za-z]{1,20}" class="form-control" placeholder="Exemple: 'Monkey'" aria-label="Username" aria-describedby="basic-addon1">
                        </div>
                    </div>
                </div>
                <button v-if="nbRound == 1" class="mt-3 col-2 btn btn-lg btn-success" @click="guess()">Guess !</button>
                <div v-else class="my-5 row justify-content-md-center">
                    <div v-if="isCorrect" class="alert alert-success col-lg-4 col-sm-12">
                        <h3>Good job !</h3>
                        <button class="btn btn-md btn-success" @click.prevent="next()">Next sound</button>
                    </div>
                    <div v-if="isWrong && nbRound === 2" class="alert alert-warning col-lg-4 col-sm-12">
                        <h3>You have a second chance, try again !</h3>
                        <button class="btn btn-md btn-secondary" @click.prevent="guess()">Guess again</button>
                    </div>
                    <div v-if="isWrong && nbRound === 3" class="alert alert-danger col-lg-4 col-sm-12">
                        <h3>Too bad... Better luck next time</h3>
                        <h4>The correct word was <span class="bold">{{sounds[theme][currentIndex]}}</span></h4>
                        <button class="btn btn-md btn-secondary" @click.prevent="next()">Next sound</button>    
                    </div>
                </div>
            </div>
        </div>
        <div v-else class="row justify-content-md-center">
            <div class="col-6 py-5 my-5 alert alert-warning" v-if="gameOver">
                <h1 v-if="score > sounds[theme].lenght / 2">Finished ! Good job !</h1>
                <h1 v-else>Game over... Try again !</h1>
                <h2 class="mt-3">Ton score est de : {{ score }} / 10</h2>
                <button class="btn btn-primary mt-3" @click="refresh()">Back to home !</button>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name: 'Sound',
    data() {
        return {
            sounds: {
                animals: ['dog','cat','goat','cicada','cricket','donkey','duck','elephant','sheep','wolf'],
                commons: ['car','doorbell','fireworks','siren','plane','helicopter'],
                instruments: ['cello','drums','guitar','piano','trumpet','violin'],
                },
            currentIndex: 0,
            soundGo: false,
            score: 0,
            nbRound: 1,
            theme: String,
            isCorrect: false,
            isWrong: false,
            newRound: true,
            gameOver: false,
            choseTheme: false,
        }
    },
    mounted (){
        this.sounds.animals.sort( () => Math.random() - 0.5);
        this.sounds.commons.sort( () => Math.random() - 0.5);
        this.sounds.instruments.sort( () => Math.random() - 0.5);
    },
    methods: {
        hasChoseTheme(theme){
            this.theme = theme;
            this.choseTheme = true;
        },
         guess() {
            var userGuess = document.getElementById('user-guess');
            
            if(this.sounds[this.theme][this.currentIndex] == userGuess.value){
                this.score++;
                this.isCorrect = true;
                this.isWrong = false;
            }
            else{
                this.isCorrect = false;
                this.isWrong = true;
            }

            // If game is over
            if(this.currentIndex == this.sounds[this.theme].length-1){
                this.gameOver = true;
            }
            this.nbRound++;
        },
        next(){
            this.isCorrect=false;
            this.isWrong=false;
            this.currentIndex++;
            this.nbRound = 1;
            document.getElementById('user-guess').value = '';
        },
        launchSound(){
            this.soundGo = true;
        },
        shuffle(array) {
            var currentIndex = array.length, temporaryValue, randomIndex;
            while (0 !== currentIndex) {
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;
                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;
            }
            return array;
        },
        refresh(){
            location.reload();
        }
    }
}

</script>

<style scoped>
.white{
    color: white;
}
.black{
    color: black;
}
.bold{
    font-weight: 800;
}
.bitbold{
    font-weight: 600;
}
</style>