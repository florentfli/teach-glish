<template>
    <div>
        <b-navbar toggleable="lg" type="dark" variant="secondary">
            <b-navbar-brand class="mx-5" href="#">Teach'Glish</b-navbar-brand>

            <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

            <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav>
                    <b-nav-item href="#" @click="clickHome()" id="home" class="active">Home</b-nav-item>
                    <b-nav-item href="#" @click="clickSound()" id="listnguess">List'n Guess</b-nav-item>
                    <b-nav-item href="#" @click="clickDrag()" id="synofun">Syno'Fun</b-nav-item>
                    <b-nav-item href="#" @click="clickBac()" id="scattergories">Scattergories</b-nav-item>
                </b-navbar-nav>
            </b-collapse>
        </b-navbar>
    </div>
</template>

<script>
export default {
    name: 'header',
    methods: { 
        toggleCollapse(ref) {
            let show = this.classArr.indexOf('show')>-1?false:'show'
            this.classArr = ['collapsing']
            setTimeout(() => {
                if (show){
                    let height = this.$refs[ref].firstChild.clientHeight + 'px';
                    this.styleObj = { height }
                }
                else {
                    this.styleObj = {}  
                }
            }, 10)
            setTimeout(() => {
                this.classArr = ['collapse', show]
            }, 340)
        },
        clickHome(){
            this.$emit('clickedHome');
            this.resetStyle();
            document.getElementById("home").classList.add("active");
        },
        clickSound(){
            this.$emit('clickedSound');
            this.resetStyle();
            document.getElementById("listnguess").classList.add("active");
        },
        clickDrag(){
            this.$emit('clickedDrag');
            this.resetStyle();
            document.getElementById("synofun").classList.add("active");
        },
        clickBac(){
            this.$emit('clickedBac');
            this.resetStyle();
            document.getElementById("scattergories").classList.add("active");
        },
        resetStyle(){
            document.getElementById("home").classList.remove("active");
            document.getElementById("listnguess").classList.remove("active");
            document.getElementById("scattergories").classList.remove("active");
            document.getElementById("synofun").classList.remove("active");
        }
    }
}
</script>   
<style scoped>
.bg-secondary{
    background-color: rgb(66, 66, 66) !important;
}
.active a{
    color: white !important;
}
</style>