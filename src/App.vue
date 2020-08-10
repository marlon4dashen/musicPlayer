<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>

    <main> 
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{current.artist }}</span></h2>

        <div class="control">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section> 
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)"  class="(song.src == current.src)? 'song playing' : 'song'">
        {{song.title}} - {{song.artist}}
        </button>
      </section>
    </main>
  </div>
  
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      current: {
        title: 'Song title'
      },
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Grateful',
          artist: 'Neffex',
          src: require('./assets/neffex-grateful.mp3')
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require('./assets/deaf-kev-invincible.mp3')
        }
      
      ],

      player: new Audio()
      

    }
  },

  methods: {
    play(song) {
       if(typeof song.src != "undefined"){
         this.current = song;

         this.player.src = this.current.src;
       }

       this.player.play();
       this.isPlaying = true;
       this.player.addEventListener('ended', function() {
         this.next();
       }.bind(this));
    },

    pause(){
      this.player.pause();
      this.isPlaying = false;
    },

    prev(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);


    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;

  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: black;
  color:white;
}

main{
  width: 100%;
  max-width: 768px;
  height: 100vh;
  margin: 0 auto;
  padding: 25px;
  background: linear-gradient(to right, rgb(52, 235, 113), rgb(52, 128, 235))
}

.song-title{
  color: black;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span{
  font-weight: 400;
  font-style: italic
}

.control{
  padding: 40px 15px;
  display: flex;
  align-items: center;
  justify-content: space-around;

}

button{

  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover{
  opacity: 0.8;
}

.play, .next, .prev, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: rgb(235, 201, 52);

}

.playlist{
  padding: 50px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}

.playlist h3{
  color: black;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  padding-bottom: 30px;
}

.playlist button{
  font-size: 20px;
  font-weight: 700;
  margin: 30px;
  padding: 15px;
  background-color: rgb(213, 52, 235);
  border-radius: 8px;
}

</style>
