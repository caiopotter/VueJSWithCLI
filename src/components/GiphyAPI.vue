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

        <!-- <div v-show="!loaded">
            <ul>
                <li style="display:inline-block" v-for="gif in gifs" v-bind:key="gif.id">
                    <div>
                        <svg style="margin:2px;" v-bind:width="gif.images.fixed_height_small.width" v-bind:height="gif.images.fixed_height_small.height">
                            <rect v-on:load="handleLoad" v-bind:width="gif.images.fixed_height_small.width" v-bind:height="gif.images.fixed_height_small.height" style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)" />  
                        </svg>
                    </div>
                </li>
            </ul>
        </div> -->

        <div>
            <ul>
                <li class="gif-item" v-for="gif in gifs" v-bind:key="gif.id">
                    
                    <div v-bind:style="buildGifPlaceholder(gif)" v-if=true>
                        <img style="position: absolute; z-index: 10;" v-cloak v-bind:src="gif.images.fixed_height_small.url" alt="">
                 
                    <svg style="z-index:-1" v-bind:width="gif.images.fixed_height_small.width" v-bind:height="gif.images.fixed_height_small.height">
                    </svg>
                        
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
        imgLoaded: 0,
        loaded: false,
        result: ""}
    },
    mounted() {
        axios.get("http://api.giphy.com/v1/gifs/trending?api_key=BnHDTmMLviwT2Jhowt3GNZsFATVXxR2V")
        .then(response => {
            this.gifs = response.data.data;
        });
    },
    updated(){
        console.log("updated");
    },

    methods: {
        buildURL: function(){
            let texto = this.replaceSpaces(this.textoBusca);
            return "http://api.giphy.com/v1/gifs/search?q=" + texto + "&api_key=" + this.apikey + "&limit=" + this.limit;
        },
        buildGifPlaceholder: function(gif){
            this.result = "margin: 2px; width: " + 
            gif.images.fixed_height_small.width + "; height: " + gif.images.fixed_height_small.height + "; background-color: black;";
            return this.result;
        },
        buscar: function(event){
            axios.get(this.buildURL())
            .then(response => {this.gifs = response.data.data})
        },
        replaceSpaces: function(string){
            return string.replace(" ", "+");
        },
        handleLoad: function(todo){
            todo.target.style.visibility = "visible";
            this.loaded=false;
    	    this.imgLoaded++;
            if(this.imgLoaded === this.gifs.length) {
              console.log('all image loaded')	
              this.loaded=true;
            }
        },

            
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

    .gif-placeholder{

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