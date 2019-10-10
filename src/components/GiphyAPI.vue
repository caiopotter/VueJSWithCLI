<template>
    <div id="app">
        <h1>{{ title }}</h1>
        
        <div class="wrap">
            <div class="search">
                <input v-model="textoBusca" type="text" class="searchTerm" placeholder="What are you looking for?">
                <button v-on:click="buscar" type="submit" class="searchButton">
                    <i class="fa fa-search"></i>
                </button>
                <select v-model="limit">
                    <option value="5">5</option>
                    <option value="10">10</option>
                    <option value="20">20</option>
                    <option value="50">50</option>
                </select>
            </div>
        </div>

        <div v-cloak>
            <ul>
                <li class="gif-item" v-for="gif in gifs" v-bind:key="gif.id">
                    <div style="margin:2px; background-color: black">
                        <img v-if="imgLoaded == 25" v-cloak @load="handleLoad" v-bind:src="gif.images.fixed_height_small.url" alt="">
                        <p v-else>{{imgLoaded}}</p>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'GiphyAPI',
    props: {
        msg: String
    },
    data() {
        return{
        title: "Teste Vue com Cli",
        apikey: "BnHDTmMLviwT2Jhowt3GNZsFATVXxR2V",
        textoBusca: "",
        limit: 5,
        gifs: [],
        imgLoaded: 0}
    },
    mounted() {
        axios.get("http://api.giphy.com/v1/gifs/trending?api_key=BnHDTmMLviwT2Jhowt3GNZsFATVXxR2V")
        .then(response => {this.gifs = response.data.data})
    },
    updated(){
        console.log("updated");
    },

    methods: {
        buildURL: function(){
            let texto = this.replaceSpaces(this.textoBusca);
            return "http://api.giphy.com/v1/gifs/search?q=" + texto + "&api_key=" + this.apikey + "&limit=" + this.limit;
        },
        buscar: function(event){
            axios.get(this.buildURL())
            .then(response => {this.gifs = response.data.data})
        },
        replaceSpaces: function(string){
            return string.replace(" ", "+");
        },
        handleLoad: function(todo){
    	    this.imgLoaded++;
            if(this.imgLoaded === 25) {
      	    alert('all image loaded')	
      }
    }
            
    }
}
</script>

<style>
    @import url(https://fonts.googleapis.com/css?family=Open+Sans);
    [v-cloak] { display:none; }
    [v-cloak]::before { content: "loading..."; }
    body{
    background: #f2f2f2;
    font-family: 'Open Sans', sans-serif;
    }

    .gif-item{
        display: inline-block
    }

    .search {
    width: 100%;
    position: relative;
    display: flex;
    }

    .searchTerm {
    width: 100%;
    border: 3px solid #00B4CC;
    border-right: none;
    padding: 5px;
    height: 20px;
    border-radius: 5px 0 0 5px;
    outline: none;
    color: #9DBFAF;
    }

    .searchTerm:focus{
    color: #00B4CC;
    }

    .searchButton {
    width: 40px;
    height: 36px;
    border: 1px solid #00B4CC;
    background: #00B4CC;
    text-align: center;
    color: #fff;
    border-radius: 0 5px 5px 0;
    cursor: pointer;
    font-size: 20px;
    }

    /*Resize the wrap to see the search bar change!*/
    .wrap{
    width: 30%;

    top: 50%;
    left: 50%;
    transform: translate(100%, -50%);
    }

</style>