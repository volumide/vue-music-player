<template>
<div>
    <!-- <audio :src="currentSong.src"  ref="player" autoplay></audio> -->
    <p class="logo">Mosic Player</p>
    <div class="sounds">
        <div v-for="(song, index) in allsongs" :key="index" class="artist">
             <small>
                 {{song.artist}}
            </small>
            <p>{{song.title}} <i class="fa fa-music" v-if="artist === song.artist" aria-hidden="true"></i></p>  
        </div>
       
    </div>
    <div class="controls">
        <div class="nowplaying">
            <p><span style="color:#fff">{{title}}</span> <br> <span class="artist" style="font-size:small;">{{artist}}</span> </p>
           
        </div>
        <div class="button">
            <button @click="prev"><i class="fa fa-step-backward" aria-hidden="true"></i></button>
            <button @click="loop(-5)"  :disabled ="currentTime === 0" ><i class="fa fa-backward" aria-hidden="true"></i></button>
            <button v-if="!isPlaying" @click="play" id='play'><i class="fa fa-play" aria-hidden="true"></i></button>
            <button v-else @click="pause"><i class="fas fa-pause" aria-hidden="true"></i></button>
            <button  @click="loop(5)" :disabled ="currentTime === 0"  ref="next"><i class="fa fa-forward" aria-hidden="true"></i></button>
            <button @click="next"> <i class="fa fa-step-forward" aria-hidden="true"></i></button>
        </div>
    </div>

</div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                allsongs: [
                    {
                        artist: 'Terri',
                        title: 'Come Around',
                        src: require('./assets/music/TerriComeAround.mp3')
                    },
                    {
                        artist: 'Bella Shmurda',
                        title: 'Rush',
                        src: require('./assets/music/Bella-Shmurda-Rush.mp3')
                    }
                ],
                currentSong: '',
                isPlaying: false,
                index: 0, 
                currentTime: 0,
                artist: '',
                title: '',
                player: new Audio()
            }
        },
        methods: {
            next(){
                this.index+=1
                if(this.index > this.allsongs.length - 1) {
                    this.currentTime = 0
                    this.index = 0
                }
                setTimeout(() => {
                    this.currentTime = 0
                    this.currentSong = this.allsongs[this.index]
                    this.play(this.currentSong)
                }, 1000);
            }, 
            shuffle(){

            },

            prev(){
                this.index--
                this.allsongs.length
                if (this.index < 0) {
                    this.index = this.allsongs.length - 1
                    this.currentTime = 0
                }
                this.currentSong = this.allsongs[this.index]
                this.play(this.currentSong)
            },

            async play(song){
                if (typeof song.src !== "undefined") {
                    this.currentSong = song
                    this.player.src = this.currentSong.src
                    this.artist = this.currentSong.artist
                    this.title = this.currentSong.title
                }
                this.player.currentTime = this.currentTime
                await this.player.play()
                setInterval(() => {
                    this.currentTime = this.player.currentTime
                }, 1000);

                this.player.addEventListener('ended',function(){
                    this.next()
                    return
                }.bind(this))
                // console.log(this.index)

            
                this.isPlaying = true
            },

            pause(){
                this.player.pause()
                this.isPlaying = false
            },

            loop(value){
                this.currentTime += +value
                if (this.currentTime < 0) {
                    this.currentTime = 0
                }
                this.play(this.currentSong.src)
            }
        },
        created() {
            this.currentSong = this.allsongs[this.index]
            this.player.src = this.currentSong.src
            this.artist = this.currentSong.artist
            this.title =  this.currentSong.title
            
            // this.player.play()
        },
    }
</script>

<style>
    *{
        box-sizing: border-box;
        margin:0;
        padding: 0;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif
    }
    .fa-music{
        color: rgb(230, 161, 12);
    }
    .logo{
        padding : 20px;
        font-weight: bold;
        font-size: 1.5rem;
        text-align: center;
        font-family: 'Montez', cursive;
    }
    .controls{
        padding: 20px;
        position: fixed;
        left: 50%;
        bottom: 0;
        transform: translateX(-50%);
        background: rgb(0, 0, 0);
        width: 100%;
        text-align: center;
        color:#fff

    }
    .controls p{
        font-weight: 500;
    }
    button{
        padding: 12px 25px;
        border: 1px solid transparent;
        font-weight: 200;
        background: transparent;
        font-size: 1.6rem;
        cursor: pointer;
        color: #fff;
    }
    .nowplaying {
        padding: 12px;
        font-size: 1.1rem;
        font-weight: bold;
        color: gold;
    }
    .sounds{
        margin: 30px auto;
        width: 500px;
        max-width: 80%;
        padding: 0 30px;
        border-radius: 10px;
        border: 1px solid transparent;
        box-shadow: 0 0 10px rgba(255, 215, 0, .5)
    }
    .sounds .artist{
        padding: 10px;
        margin: 3px 0;
        cursor: pointer;
        font-weight: 500;
        
    }
    .sounds .artist p{
    font-weight: 600;
    }
    .sounds .artist small{
        color: rgb(87, 87, 87);
    }
</style>
