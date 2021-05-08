<template>
    <div class="container pt-1">
        <h1 class="my-3 bold">Home</h1>
        <h2 class="my-5">Welcome to <b>Teach'Glish</b>, the fun way to teach yourself english !</h2>
        
        <div class="row justify-content-sm-center">
          <div class="col-lg-4 col-sm-12 mx-3 mb-3">
            <div class="card border-dark mb-3">
              <div class="card-header bit-bold">Learning with fun</div>
              <div class="card-body text-dark">
                <p class="card-text">We beleive learning shouldn't be something boring. That's why we want to teach you english in fun ways !</p>
              </div>
            </div>
          </div>
          <div class="col-lg-4 col-sm-12 mx-3">
            <div class="card border-dark mb-3">
              <div class="card-header bit-bold">Games for all age</div>
              <div class="card-body text-dark">
                <p class="card-text">Because there is no tight or wrong age to start learning, we embrace all age group and made learning accessible to all !</p>
              </div>
            </div>
          </div>
          
        </div>
    </div>
</template>

<script>
export default {
  name: 'home',
}
</script>

<style scoped>
.white{
    color: white;
}
.bold{
    font-weight: 800;
}
.bit-bold{
    font-weight: 600;
}
</style>