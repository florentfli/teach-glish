<template>
    <div class="container">
        <h1 class="title">COUCOU TOI</h1>
        <button class="btn btn-lg btn-primary mx-3" @click="switchGame('drag')">Drag and drop</button>
        <button class="btn btn-lg btn-primary mx-3" @click="switchGame('sound')">sound</button>

        <drag v-if="showDrag"/>
        <sound v-if="showSound"/>
    </div>
</template>

<script>
import Drag from './DragNDrop.vue'
import Sound from './Sound.vue'

export default{
    name: 'body',
    components: {
        Drag,
        Sound
    },
    methods: {
        switchGame(e){
            if(e === 'drag'){
                this.showDrag = true;
                this.showSound = false;
            } else if(e === 'sound'){
                this.showDrag = false;
                this.showSound = true;
            }
        }
    },
    data(){
        return{
            showDrag: true,
            showSound: false,
        }
    }
}
</script>
