<template>
  <div class="containerPokes">
      <Preload :visible="preloadVisible" />
      <div class="containerSearchPokes">
        <search-bar />
      </div>
      <div class="containerView">
        <router-view></router-view>
      </div>
     <div class="containerFooter" v-if="$store.state.beResults">
        <Footer/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Preload from '@/components/Preload.vue'
import SearchBar from '@/components/SearchBar.vue'
// import PokeList from '@/components/PokeList.vue'
// import NotFound from '@/components/NotFound.vue'
import Footer from '@/components/Footer.vue'
const axios = require('axios');
export default {
  name: 'Pokemons',
  components: {
    Preload,
    SearchBar,
    Footer
    // PokeList,
    // NotFound,
  },
  mounted(){
   
    setTimeout(()=>{
      this.getPokes();
    },1000)
  },
  data(){
    return {
      preloadVisible: true,
      listPokes: [],
    }
  },
  methods:{
    getPokes: async function(){
        try{
          let result = await axios.get('https://pokeapi.co/api/v2/pokemon');
          result = result.data.results.map((item)=>{
              item.favorite = false;
              return item;
          })
          this.$store.commit('getPokemons',result);
          this.preloadVisible = false;
        }catch(err){
          console.log("Error: ", err);
          this.preloadVisible = false;
        }
    }
  },
  camputed:{
    
  }
}
</script>
<style>
.containerPokes{
  display: grid;
  height: 100vh;
  grid-template-areas: ". containerSearchPokes ."
                       ". containerView ."
                       "containerFooter containerFooter containerFooter";
  grid-template-columns: minmax(30px, 1fr) minmax(300px, 570px)  minmax(30px, 1fr);
  grid-template-rows: 125px 1fr 80px;
}
.containerPokes .containerSearchPokes{
  grid-area: containerSearchPokes;
  display: grid;
  align-content: center;
}
.containerPokes .containerView{
  grid-area: containerView;
  overflow: auto;
}
.containerPokes .containerView::-webkit-scrollbar {
    -webkit-appearance: none;
}

.containerPokes .containerView::-webkit-scrollbar:vertical {
    width:10px;
}

.containerPokes .containerView::-webkit-scrollbar-button:increment,.containerPokes .containerView::-webkit-scrollbar-button {
    display: none;
} 

.containerPokes .containerView::-webkit-scrollbar:horizontal {
    height: 10px;
}

.containerPokes .containerView::-webkit-scrollbar-thumb {
    background-color: #797979;
    border-radius: 20px;
    border: 2px solid #f1f2f3;
}

.containerPokes .containerView::-webkit-scrollbar-track {
    border-radius: 10px;  
}
.containerPokes .containerFooter{
  grid-area: containerFooter;
}

</style>