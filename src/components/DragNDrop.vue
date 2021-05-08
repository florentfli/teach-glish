<template>
    <div class="drag pt-1">
        <h1 class="mt-3 bold">Syno'Fun</h1>
        <div v-if="!gameOver">
            <div v-if="!dragGo">
                <h3 class="my-5">Find the words coresponding to the phrases according to you selected theme !</h3>
                <button class="btn btn-lg btn-success" @click.prevent="launchDrag()">Start !</button>
            </div>
            <div v-else>
                <div v-if="choseTheme" class="mb-5">
                    <div class="row justify-content-sm-center">
                        <h2 class="col-lg-4 col-sm-6 my-4 alert alert-secondary">Score : <span class="bold">{{ score }}/{{ mots[theme].length }}</span></h2>
                    </div>
                    <h4 class="mb-4">Find the coresponding word described by the phrase :</h4>
                    <h1>Phrase: <span class="bitbold black">{{currentPhrase.phrase}}</span></h1>
                    <div v-show="!success && !fail" class="my-4 row justify-content-sm-center">
                        <div v-for="mot in mots[theme]" :key="mot.value" class="m-3 btn-group-vertical col-lg-2 col-sm-10" >
                            <button :id="`btn-`+mot.value" type="button" 
                            class="btn btn-secondary btn-lg white" 
                            @click.prevent="clickButton(mot)">{{mot.value}}</button>
                        </div>
                    </div>
                </div>
                <div v-else>
                    <h2 class="my-5">Chose a theme</h2>
                    <div class="row mt-5 justify-content-md-center">
                        <button class="col-2 mx-3 btn btn-lg btn-primary" @click.prevent="hasChoseTheme('animals')">Animals</button>
                        <button class="col-2 mx-3 btn btn-lg btn-primary" @click.prevent="hasChoseTheme('object')">Object</button>
                        <button class="col-2 mx-3 btn btn-lg btn-primary" @click.prevent="hasChoseTheme('celebrity')">Celebrity</button>
                    </div>
                </div>
            </div>
            <div class="row mt-3 justify-content-md-center">
                <div v-if="success" class="col-6 alert alert-success pt-4">
                    <h3>Your answer : <span class="bitbold">{{userChoice.value}}</span></h3>
                    <h2>Congrats ! Keep going !</h2>
                    <button @click.prevent="hideResult()" class="btn btn-secondary my-3">Next phrase</button>
                </div>
                <div v-if="fail" class="col-6 alert alert-danger pt-4">
                    <h3>Your answer : <span class="bitbold">{{userChoice.value}}</span></h3>
                    <h2>Wrong ! The good answer was <b>{{ currentPhrase.value}}</b></h2>
                    <button @click.prevent="hideResult()" class="btn btn-secondary my-3">Next phrase</button>
                </div>
            </div>
        </div>
        <div class="row justify-content-md-center" v-else>
            <div v-if="score >= mots[theme].length / 2" class="col-6 px-5 py-4 my-4 alert alert-success">
                <h1 class="bitbold">Game over !</h1>
                <h2 class="my-4">Your score is <span class="bitbold">{{score}}/{{mots[theme].length}}</span> !</h2>
                <button class="btn btn-lg btn-primary m-3" @click.prevent="reset()">Play Again</button>
                <button class="btn btn-lg btn-secondary m-3" @click.prevent="refreshPage()">Back to the menu</button>
            </div>
            <div v-else class="col-6 px-5 py-4 my-4 alert alert-danger">
                <h1 class="bitbold">Game over...</h1>
                <h2 class="my-4">Your score is <span class="bitbold">{{score}}/{{mots[theme].length}}</span>...</h2>
                <button class="btn btn-lg btn-primary m-3" @click.prevent="reset()">Play Again</button>
                <button class="btn btn-lg btn-secondary m-3" @click.prevent="refreshPage()">Back to the menu</button>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name: 'drag',
    data() {
        return {
            gameOver: false,
            dragGo: false,
            score: 0,
            cursor: 0,
            success: false,
            fail: false,
            choseTheme: false,
            theme: String,
            mots: {
                animals: [
                    {
                        value: "Dog",
                        phrase: "I am the best friend of humans and i bark to say something."
                    },
                    {
                        value: "Cat",
                        phrase: "I am cute and fluffy, i purr when i'm happy."
                    },
                    {
                        value: "Chicken",
                        phrase : "Humans love me for my eggs and to fry me."
                    },
                    {
                        value: "Cow",
                        phrase : "Humans love to milk me and eat my meat."
                    },
                    {
                        value: "Monkey",
                        phrase : "I jump and move from tree to tree using lianas."
                    },
                    {
                        value: "Mouse",
                        phrase : "I am small but humans are scared of me. I also like cheese a lot."
                    },
                    {
                        value: "Whale",
                        phrase : "I am the biggest animal family in the world."
                    },
                    {
                        value: "Eagle",
                        phrase : "I can fly and i represent the USA."
                    },
                    {
                        value: "Horse",
                        phrase : "I am tall and elegent, i move people on my back or carry things for them."
                    }
                ],
                object: [
                    {
                        value: "Plane",
                        phrase: "Fly in sky to transport people or marchandise to point A to B"
                    },
                    {
                        value: "Bike",
                        phrase: "2 wheels transportation type moved by the legs and drive with a handlebar"
                    },
                    {
                        value: "Car",
                        phrase : "4 wheels powered by a fuel engine, capable to drive severals hundreds of miles with one tank of fuel"
                    },
                    {
                        value: "Knife",
                        phrase : "I have a handle, 2 edges but one only is sharp. I am on every table."
                    },
                    {
                        value: "Tree",
                        phrase : "I am big and tall, people use me to make furnitures, houses and crafts. I often lives with many other green friends."
                    },
                    {
                        value: "Computer",
                        phrase : "Machinery capable of calculating and displaying informations. I only understand ones and zeros."
                    },
                    {
                        value: "Money",
                        phrase : "I am phisycal and virtual. People work all their lives to have me. I don't produce happiness."
                    },
                    {
                        value: "Botle",
                        phrase : "People open me and close me all the time. My only purpose is to contain liquid. I exist in different materials."
                    }
                ],
                celebrity: [
                    {
                        value: "Mickeal Jordan",
                        phrase: "Famous NBA player who finished his carreer as a ALL-STAR in the Chicago Bulls"
                    },
                    {
                        value: "Donald J. Trump",
                        phrase: "Former USA president who had a orange skin tone and appears in the movie Home Alone, and in a wrestling show"
                    },
                    {
                        value: "Manuel Macron",
                        phrase : "Current president of France"
                    },
                    {
                        value: "Cristiano Ronaldo",
                        phrase : "Consider world best soccer player. Currently playing in the club Juventus"
                    },
                    {
                        value: "Elon Musk",
                        phrase : "He is the founder of SpaceX; early stage investor,and product architect of Tesla"
                    },
                    {
                        value: "Abraham Lincoln",
                        phrase : "Famous actor that played the role of Iron Man"
                    },
                    {
                        value: "Tom Cruise",
                        phrase : "One of the most paid actor in the world, he played the main role in Top Gun and Mission Impossible"
                    },
                    {
                        value: "Julius Caesar",
                        phrase : "He was a Roman general and statesman who played a critical role in the events that led to the demise of the Roman Republic and the rise of the Roman Empire."
                    },
                    {
                        value: "Barack Obama",
                        phrase : "He is an American politician and attorney who served as the 44th president of the United States from 2009 to 2017"
                    }
                ]
            },
            currentPhrase: Object,
            userChoice: Object
        }
    },
    mounted() {
        this.mots.animals = this.mots.animals.sort( () => Math.random() - 0.5);
        this.mots.object = this.mots.object.sort( () => Math.random() - 0.5);
        this.mots.celebrity = this.mots.celebrity.sort( () => Math.random() - 0.5);

        this.userChoice = null;
    },
    methods: {
        hasChoseTheme(theme){
            this.theme = theme;
            this.currentPhrase = this.mots[this.theme][this.cursor];
            this.choseTheme = true;
        },
        launchDrag(){
            this.dragGo = true;
        },
        resetBtn(){
            for(let i=0; i < this.mots[this.theme].length; i++){
                var btn = document.getElementById("btn-" + this.mots[this.theme][i].value)
                btn.classList.remove("btn-success")
                btn.classList.remove("btn-danger")
                btn.classList.add("btn-secondary")
            }
        },
        hideResult(){
            this.fail = false;
            this.success = false;
            if(this.cursor == this.mots[this.theme].length-1) this.gameOver = true;
            else this.nextMot();
        },
        clickButton(mot){
            this.userChoice = mot
            var btn = document.getElementById("btn-"+mot.value)
            this.resetBtn();

            if(mot.phrase === this.currentPhrase.phrase){
                this.success = true;
                btn.classList.remove("btn-secondary")
                btn.classList.add("btn-success")
                
                this.score++;
            } else {
                this.fail = true;
                btn.classList.remove("btn-secondary");
                btn.classList.add("btn-danger");
            }
        },
        nextMot(){
            this.resetBtn();
            this.cursor++;
            this.currentPhrase = this.mots[this.theme][this.cursor];
        },
        shufflePhrase(array) {
            var currentIndex = array.length, temporaryValue, randomIndex;
            while (0 !== currentIndex) {
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;
                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;
            }
            console.log("ALLALAL")
            return array;
        },
        refreshPage(){
            location.reload()
        }
    }

    // Phrase aléatoire 
    // Incrémentation du score quand c'est juste
    // Réinitialiser en secondary les buttons quand questions
    // Compter le nombre de tour en fonction taille tableau
    // Ecran de fin de partie
    // Ajouter des thèmes
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