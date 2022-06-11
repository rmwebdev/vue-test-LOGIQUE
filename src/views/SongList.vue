<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            ngmusic
          </v-list-item-title>
        </v-list-item-content>
        <v-btn class="ma-2" color="darken-2" dark @click="drawer = !drawer">
          <v-icon dark left>
            mdi-arrow-left
          </v-icon>
        </v-btn>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app class="text-center">
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-spacer></v-spacer>
      <h2>ngmusic</h2>
      <v-spacer></v-spacer>
      <v-btn icon @click="vDialog">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <div class="text-center" v-if="loading">
    <v-progress-circular
      :value="100"
      color="blue-grey"
    ></v-progress-circular>
    </div>

    <div v-else>
      <v-col cols="12">
        <div class="text-center">
          <span>Search result for: </span><span class="artist__name">{{ name }}</span>
        </div>
        
      </v-col>
        <v-col
         v-for="(list,index) in lists" :key="index"
          cols="12"
        >
          <v-card
           class="card__lists"
          >
            <div class="d-flex">
              <v-avatar
                class="ma-3"
                size="130"
                rounded
              >
                <v-img :src="list.artworkUrl100" class="images"></v-img>
              </v-avatar>
              <v-row dense style="padding-top: 6px">
                <v-col cols="12">
                    <h5>{{list.artistName}}</h5>
                    <h3>{{list.trackCensoredName}}</h3>
                </v-col>
                <v-col cols="7">
                  <button class="genre">{{list.primaryGenreName}}</button>
                </v-col>
                <v-col cols="4">
                  <v-row align="center" justify="end">
                      <v-list-item-avatar color="">
                        <v-img
                          class="elevation-6"
                          alt=""
                          src="../assets/currency-dollar@2x.png"
                        ></v-img>
                      </v-list-item-avatar>
                      <span
                        class=""
                        style="
                          font-weight: bold !important;
                          font-size: 1em !important;
                          color: #f5b014 !important;
                        "
                      >
                        {{list.collectionPrice}}
                      </span>
                    </v-row>
                </v-col>
              </v-row>
            </div>
          </v-card>
        </v-col>
        <div class="text-center mt-4">
          <v-btn
            style="
              height: 50px;
              padding: 20px;
              border-radius: 25px;
              background-color: #e2e8f0;
            "
            @click="loadMore"
          >
            Load More
          </v-btn>
        </div>


    </div>
    </v-main>
      <v-dialog
        v-model="dialog"
        fullscreen
        hide-overlay
      >
        <div class="relative" style="position: fixed;width: 100%;height: 100%;background: rgba(0,0,0,0.7);">
          <div class="text-center title-search">
          <h2>Search</h2>
          </div>

          <div class="text-center group__input">
            <div class="button__group">
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
            <div class="search__group mb-2">
              <form v-on:submit.prevent="onSearch">
               <input type="text" v-model="search" class="searchTerm" placeholder="Search" />
              </form>
            </div>
                  
                  
          </div>
        </div>
          
        
      </v-dialog>

  </v-app>
</template>

<script>

import axios from 'axios'
  export default {
    data: () => ({
      drawer: null,
      loading: false,
      dialog:false,
       items: [
          { title: 'Dashboard', icon: 'mdi-view-dashboard' },
          { title: 'Song List', icon: 'mdi-help-box' },
        ],
        lists: [],
        limits: 5,
        name: '',
        search:'',
      }),

     mounted() {
       this.name = this.$route.params.name
       this.getData(this.name, this.limits)
     },
      methods:{
          async getData(name, limits){
            let limit = `&limit=${limits}`
                try {
                  this.loading = true
                  const {data}  = await axios({
                    url: `https://itunes.apple.com/search?term=${name}${limit}`,
                    method: 'GET',
                  })
                  this.lists = data.results
                  this.loading = false
                  this.dialog = false
                  console.log(this.lists)
                } catch (error) {
                  console.log(error)
                  this.loading = false
                }
          },

          onSearch(){
            this.name = this.search
          if(this.search){
            this.getData(this.search, this.limits)
          }
        },
        async loadMore(){
          this.lists = []
         try {
                  this.loading = true
                  const {data}  = await axios({
                    url: `https://itunes.apple.com/search?term=${this.name}&country=ca`,
                    method: 'GET',
                  })
                  this.lists = data.results
                  this.loading = false
                  this.dialog = false
                  console.log(this.lists)
                } catch (error) {
                  console.log(error)
                  this.loading = false
                }
        },
        vDialog(){
          this.search = ''
          this.name = ''
          this.dialog = true
        }
      }
  }
</script>
<style lang="scss" scoped>
.card__lists:hover {
    transition: all 0.2s ease-out;
    box-shadow: 0px 4px 8px rgba(38, 38, 38, 0.2);
    border: 1px solid #cccccc;
    background-color: white;
    cursor: pointer;
  
}

.button__load-more {
  height: 50px;
  padding: 37px;
  border-radius: 25px;
  background-color: #e2e8f0;
}
.shape {
  width: 40px;
  height: 40px;
  float: right;
}
.genre{
 background-color: #10b981;
 border-radius: 20px;
 color: #e2e8f0;
 padding: 10px;
}
.image{
  width: 100px;
  height: 100px;
  margin-right: 5px;
}
.images {
  border-radius: 10%;

}
.v-progress-circular {
  margin: 1rem;
}
.modal-fullscreen .modal-content {
  background: transparent;
  border: 0;
  -webkit-box-shadow: none;
  box-shadow: none;
}

.title-search {
  position: absolute; 
  left: 0; 
  right: 0; 
  top:40%;
  margin-left: auto; 
  margin-right: auto; 
  width: 100px; 
  color:white;
}
.group__input {
  position: absolute;
    left: 0; 
    right: 0; 
  
  margin-left: auto; 
  margin-right: auto; 
    top:50%;
  width:90%
}
.button__group {
   background-color: white;
    border-radius: 20px;
    padding: 10px;
    line-height: 40px;
  margin-bottom: 10px;
}

.search__group {
   background-color: white;
    border-radius: 20px;
    background-color: #712bda;
    padding: 10px;
    line-height: 40px;
    line-height: 40px;
}
.searchTerm {
  width: 90%;
  color: #fff;
  outline-color: #cccccc;
  padding-left: 10px;
}
.artist__name{
  color: #712bda;
  font-weight: bold;
  font-size: 20px;
  margin-left: 5px;
}
</style>
