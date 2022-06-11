<template>
  <div class="home relative">
    <div class="logos text-center">
      <img src="../assets/logo.svg"
     class="logos__image">
    </div>
    <div class="text-center button__group">
      <v-btn
        class="mx-2"
      text
        color="primary"
      >
        Artist
      </v-btn>
      /

      <v-btn
        class="mx-2"
        text
        color="primary"
      >
      Album
      </v-btn>
     /
      <v-btn
        class="mx-2"
        text
        color="primary"
      >
      Title
      </v-btn>
    </div>
    <div class="text-center button___search">
      <form v-on:submit.prevent="onSearch">
        <input class="form-control" type="text" v-model="search" placeholder="Search"/>
      </form>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    // HelloWorld
  },
  data: () => ({
      items: [
        {
          icon: 'mdi-wifi',
          text: 'Wifi',
        },
        {
          icon: 'mdi-bluetooth',
          text: 'Bluetooth',
        },
        {
          icon: 'mdi-chart-donut',
          text: 'Data Usage',
        },
      ],
      search:'',
    }),
    methods: {
      onSearch(){
      console.log(this.search)
        if(this.search){
          this.$router.push({
              name: 'SongList', 
              params: { name: this.search}
          });
        }
      }
    }
}
</script>
<style lang="scss" scoped>
.home {
  width: 100%;
  height: 100%;
  background-image: linear-gradient(180deg, #712bda, #a45deb);
}
.button__group {
position: absolute;
    bottom: 20%;
    left: 10%;
    right: 10%;
    background-color: white;
    border-radius: 20px;
    padding: 10px;
  line-height: 40px;

}
.button___search {
  position: absolute;
    bottom: 12%;
    left: 10%;
    right: 10%;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 20px;
    padding: 10px;
    line-height: 40px;
}
.logos{
  height: 50%;
  position: relative;
}
.logos__image{
  position: absolute;
    bottom: 0;
}
.form-control {
  width: 80%;
}
  

.mx-2 {
  padding: 20px;
}
</style>
