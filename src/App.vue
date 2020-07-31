<template>
  <div id="app">
    <div class="container">
	    <h1>Simon Says</h1>
		    <div class="simon">
			    <ul>
				    <li class="red" :class="{active:color == 'red'}" data-tile="0" @click="sound($event.target.dataset.tile)"></li>
				    <li class="blue" :class="{active:color == 'blue'}" data-tile="1" @click="sound($event.target.dataset.tile)"></li>
				    <li class="yellow" :class="{active:color == 'yellow'}" data-tile="2" @click="sound($event.target.dataset.tile)"></li>
				    <li class="green" :class="{active:color == 'green'}" data-tile="3" @click="sound($event.target.dataset.tile)"></li>
		    	</ul>
		    </div>
		    <div class="game-info">
			    <h2>Round: <span data-round="0">{{this.round}}</span></h2>
			    <button data-action="start" @click="start">Start</button>

		    </div>
		    <div class="game-options">
			    <h2>Game Options:</h2>
			    <input type="radio" name="mode" value="Easy" v-model="mode" checked>Easy<br>
			    <input type="radio" name="mode" value="Middle" v-model="mode">Middle<br>
			    <input type="radio" name="mode" value="Hard" v-model="mode">Hard<br>
		    </div>
	  </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
    color:'',
    round:0,
    myTurn:[],
    gameTurn:[],
    mode:'',
    colors:['red','blue','yellow','green'],
    int:0,
    }
  },
  watch: {
    'myTurn':{
      handler(val){
        console.log(val)
          if (val.length > 0 && this.round > 0){
            var i = this.int
            if (this.gameTurn[i] != val[i]){
              console.log('error')
              this.round = 0
              this.gameTurn = []
              this.myTurn = []
              alert('Вы проиграли :)');
          } else if (this.gameTurn[i] == val[i]) { 
            console.log('success')
            this.int++
            if (this.gameTurn.length == val.length){
              setTimeout(()=>{
                this.start()
              }, 3000)
            }
          } 
          }
        }
      }
  },
  methods:{
    sound(e){
      this.color = this.colors[e]
      var audio = new Audio(Number(e)+1+'.ogg')
      audio.play()
      setTimeout(()=>{
            this.color = ''
          },1000)
      this.myTurn.push(Number(e))
    },
    start(){
      if (this.mode == ''){
        alert('Выберите сложность')
      } else {
      this.round++
      this.gameTurn = []
      this.int = 0
      if (this.myTurn.length > 0){
        this.myTurn = []
      }
      var delay = 0;
      if (this.mode == 'Easy'){
        delay = 1500
      } else if (this.mode == 'Middle'){
        delay = 1000
      } else if (this.mode == 'Hard'){
        delay = 400
      }
      function random(min, max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min + 1)) + min;
      }
      for (var i =0 ; i <this.round; i++){
      this.gameTurn.push(random(0,3))
      }
      this.gameTurn.forEach((item, i)=>{
        setTimeout(()=>{
          this.color = this.colors[item]
          var audio = new Audio(item+1+'.ogg')
          audio.play()
          console.log(item)
          console.log(i)
          setTimeout(()=>{
            this.color = ''
          },delay/2)
        },delay * i,)
      }) 
      }
    }
  }
}
</script>

<style lang="sass">
#app 
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px

h1 
	margin: 1em 0 2em
	text-align: center


ul 
	list-style: none


ul, li 
	padding: 0
	margin: 0


p[data-action="lose"] 
	display: none


.active 
	opacity: 1 !important


.clearfix 
	width: 100%
	clear: both


.container 
	width: 540px
	margin: 0 auto


.simon 
	background: #fff
	position: relative
	float: left
	margin-right: 3em	
	width: 302px
	height: 295px
	-webkit-border-radius: 150px 150px 150px 150px;
	border-radius: 150px 150px 150px 150px
    -moz-box-shadow: 2px 1px 12px #aaa
    -webkit-box-shadow: 2px 1px 12px #aaa
    -o-box-shadow: 2px 1px 12px #aaa
    box-shadow: 2px 1px 12px #aaa


.red, .blue, .yellow, .green 
	opacity: 0.6
	height: 290px
	-webkit-border-radius: 150px 150px 150px 150px
	border-radius: 150px 150px 150px 150px
	position: absolute
	text-indent: 10000px

.red:hover, .blue:hover, .yellow:hover, .green:hover 
	border: 2px solid black


.red 
	background: #FF5643
	clip: rect(0px, 300px, 150px, 150px)
	width: 296px


.blue 
	background: dodgerblue
	clip: rect(0px, 150px, 150px, 0px)
	width: 300px


.yellow 
	background: #FEEF33
	clip: rect(150px, 150px, 300px, 0px)
	width: 300px


.green 
	background: #BEDE15
	clip: rect(150px,300px, 300px, 150px)
	width: 296px


.game-info 
	margin-top: 90px

.game-info button 
	width: 5em
	box-sizing: border-box
	font-size: 1.4em
	-webkit-border-radius: 10px 10px 10px 10px
	border-radius: 10px 10px 10px 10px
	background: #6DABE8
	border: none
	padding: 0.3em 0.6em


.game-info button:hover 
	background: #78BCFF


.game-options h2 
	margin-top: 30px
	margin-bottom: 0


.game-options input[type="radio"] 
	margin-right: 10px


.hoverable:hover 
	cursor: pointer


</style>
