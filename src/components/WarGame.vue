<template>
	<div>
		<section class="h-sreen shadow-lg p-6">
			<div class="grid sm:grid-cols-2 grid-cols-1 gap-4 mx-auto ">
				<div class="grid grid-rows-2 grid-flow-col">
					<h2 class="px-2 font-mono text-lg font-semibold">Player 1</h2>
					<div class="h-12 relative max-w-xl rounded-full overflow-hidden">
						<div
							class="w-full h-full bg-transpatrent border z-20 absolute flex items-center text-gray-500 justify-center">
							{{ playerHealth }}
						</div>
						<div id="bar" class="h-full bg-green-500 relative" :style="{ width: playerHealth + '%' }"></div>
					</div>
				</div>
				<div class="grid grid-rows-2 grid-flow-col">
					<h2 class="px-2 font-mono text-lg font-semibold">Player 2</h2>
					<div class="h-12 relative max-w-xl rounded-full overflow-hidden">
						<div
							class="w-full h-full bg-transpatrent border z-20 absolute flex items-center text-gray-500 justify-center">
							{{ monsterHealth }}
						</div>
						<div id="bar" class="h-full bg-green-500 relative" :style="{ width: monsterHealth + '%' }">
						</div>
					</div>
				</div>
			</div>
			<div class="h-24 mt-36 px-10 relative" v-if="!gameIsPlaying">
				<div class="container grid grid-cols-1 mx-auto">
					<button
            class="shadow-sm px-6 py-6 rounded text-white text-2xl bg-green-300 shadow-xl w-60 place-self-center " @click="game()"
          >
            New Game
          </button>
				</div>
			</div>
			<div class="h-42 mt-36 px-10" v-else>
				<div class="grid sm:grid-cols-4 grid-col-1 grid-row-1 gap-10 mx-auto font-mono">
					<button
            class="shadow-sm rounded-md bg-yellow-300 text-white text-xl font-extrabold flex items-center justify-center py-3 px-3" @click="help()"
          >
            Help
          </button>
					<button
            class="shadow-sm rounded-md bg-indigo-300 text-white text-xl font-extrabold flex items-center justify-center py-3 px-3" @click="superAttack()"
          >
            Super Attack
          </button>
					<button
            class="shadow-sm rounded-md bg-purple-300 text-white text-xl font-extrabold flex items-center justify-center py-3 px-3" @click="attack()"
          >
            Attack
          </button>
					<button
            class="shadow-sm rounded-md bg-red-300 text-white text-xl font-extrabold flex items-center justify-center py-3 px-3" @click="game()"
          >
            End Game
          </button>
				</div>
			</div>
			<div class="h-auto mt-36 px-10" v-if="messages.length > 0">
				<div class="container grid grid-cols-1 mx-auto">
					<ul>
						<li class="text-center text-xs font-mono" v-for="item in messages"
							:class="{'text-red-500':item.isPlayer,'text-blue-500':!item.isPlayer}">
							{{item.text}}
						</li>
					</ul>
				</div>
			</div>
		</section>
	</div>
</template>

<script>
	export default {
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsPlaying: false,
      messages:[]
    };
  },
  methods:{
    // start and end game
    game(){
      this.messages.length =0;
      this.gameIsPlaying = !this.gameIsPlaying;
      this.playerHealth = 100;
      this.monsterHealth =100;
      
    },
    help(){
        if(this.monsterHealth != 100 && this.playerHealth !=100 ){
          if(!this.done){ 
              this.done = true;
              if(this.playerHealth <= 90){
              this.playerHealth +=5;
            }
            this.monsterHealth -=this.calculateDamage(1, 5);
            }
        }
    },
    superAttack(){
      if(!this.going) {
          this.going = true;
          let damageMonster= this.calculateDamage(10, 20) ;
          let damagePlayer = this.calculateDamage(3, 10) ;
          this.monsterHealth -=damageMonster;
          this.playerHealth -=damagePlayer;
          this.logDamage(false,`player 2 Due to a severe attack`, damageMonster);
          this.logDamage(true,`player 1 Due to a severe attack`,damagePlayer);
      }
      
    },
    attack() {
        let damagePlayer= this.calculateDamage(5, 12) ;
        let damageMonster= this.calculateDamage(3, 10) ;
        this.playerHealth -= damagePlayer;
        this.monsterHealth-= damageMonster;
        this.health(this.monsterHealth,'You won ');
        this.health(this.playerHealth, 'You lose ');
        this.logDamage(false,`player 2`, damageMonster);
        this.logDamage(true,`player 1`,damagePlayer);
        
    },
    calculateDamage(min, max) {
          return Math.max((Math.floor(Math.random() * max) + 1), min);
    },
    health(item,message) {
      if (item <= 0) {
            confirm(message+',Do you want to start a new game?');
            this.playerHealth = 100;
            this.monsterHealth = 100;
            this.game();
      }
    this.messages =[];      
    },
    logDamage(bool,player,damage){
      if(this.gameIsPlaying  != false){
        this.messages.unshift({
                isPlayer: bool,
                text:`Injury to ` + player + ` = `+ damage
              })
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>