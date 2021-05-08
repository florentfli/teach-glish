<template>
    <div class="header" style="background-color:red;">
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
            <div class="container">
                <a class="navbar-brand" href="#">Teach'Glish</a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                    </button>
                <div class="collapse navbar-collapse" id="navbarResponsive">
                <ul class="navbar-nav ml-auto">
                    <li class="nav-item active">
                        <a class="nav-link" href="#" @click="clickHome()">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" @click="clickSound()">List'n Guess</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" @click="clickDrag()">Syno'Fun</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#" @click="clickBac()">Scattergories</a>
                    </li>
                </ul>
                </div>
            </div>
        </nav>
    </div>
</template>

<script>
export default {
    name: 'header',
    methods: { 
        clickHome(){
            this.$emit('clickedHome');
        },
        clickSound(){
            this.$emit('clickedSound');
        },
        clickDrag(){
            this.$emit('clickedDrag');
        },
        clickBac(){
            this.$emit('clickedBac');
        }
    }
}
</script>   