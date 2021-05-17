<template>
	<v-container fluid style="width:600px;height:600px;">
			<v-row>
				<v-col v-for="item in gameBoard3x3" :key="item.id" cols="4" style="background-image:'..assets/forest.png';border:1px ridge crimson;width:200px;height:200px;padding:1%;background-color:black;color:white">
					<div v-if="item.terrain === 'plains'" class="plainsbg">
						<p style="color:white;padding:5%">{{ item.terrain }}</p>

					</div>

					<div v-if="item.terrain === 'forest'" class="forestbg">
						<p style="color:white;padding:5%">{{ item.terrain }}</p>

					</div>

					<div v-if="item.terrain === 'mountains'" class="mountainsbg">
						<p style="color:white;padding:5%">{{ item.terrain }}</p>

					</div>

					<div v-if="item.terrain === 'ocean'" class="oceanbg">
						<p style="color:white;padding:5%">{{ item.terrain }}</p>

					</div>
					

					
					<!-- Greeting: {{ item.hello }}
					<br/>
					Row: {{ item.row }}
					<br/>
					Col: {{ item.col }}
					<br />
					Player? {{ item.residentPlayer }}
					<br />
					NPC? {{ item.residentNpc }}
					<p v-if="item.residentPlayer === true"><span style="font-size:2rem;">PLAYER</span></p>

					<p v-if="item.residentNpc === true"><span style="font-size:2rem;">NPC</span></p>

					<p v-if="item.residentNpc === true"><span style="font-size:2rem;">{{ item.npcType.name }}</span></p> -->

					<!-- <img v-if="item.terrain === 'plains'" height="185px" style="" src="../assets/plains.png" alt="">
					<img v-if="item.terrain === 'mountains'" height="185px" style="" src="../assets/mountains.png" alt="">
					<img v-if="item.terrain === 'forest'" height="185px" style="" src="../assets/forest.png" alt="">
					<img v-if="item.terrain === 'ocean'" height="185px" style="" src="../assets/ocean.png" alt=""> -->
				</v-col>
			</v-row>


			<!-- GAME BUTTONS -->
			<!-- <v-row>
			<v-col cols="12" :elevation="50">
				<button
					id="control_Btn"
					@click="move('up')" 
					v-on:keyup.up="moveUp()"
					style="border:2px ridge silver;width:180px;height:45px;margin-left:175px;background-color:tan;"
				>
					Up
				</button>
			</v-col>
			</v-row>
			<v-row>
			<v-col cols="4">
				<button 
					id="control_Btn"
					@click="move('left')"
					style="border:2px ridge silver;width:160px;height:40px;margin-left:55px;background-color:tan;"
					:elevation="hover ? 26 : 6"
				>
					Left
				</button>
			</v-col>
			<v-col cols="4">
				<button 
					id="control_Btn"
					@click="move('down')"
					style="border:2px ridge silver;width:160px;height:40px;background-color:tan;"
					:elevation="hover ? 26 : 6"
				>
					Down
				</button>
			</v-col>
			<v-col cols="4">
				<button 
					id="control_Btn"
					@click="move('right')"
					style="border:2px ridge silver;width:160px;height:40px;margin-left:-55px;background-color:tan;"
					:elevation="hover ? 26 : 6"
				>
					Right
				</button>
			</v-col>
			</v-row> -->
</v-container>
</template>

<script>
	import io from "socket.io-client"; //eslint-disable-line vue/no-unused-components

	export default {
		name: 'BlockGameNoCanvas',
		components: {
		},
		data() {
			return {
				test: "..assets/forest.png",
				// GAME BOARD RELATED STUFF
				// GAME BOARD RELATED STUFF
				// GAME BOARD RELATED STUFF
                gameBoard3x3: [
                    {
                        // BOARD INFO
						id: 0,
                        row: 1,
                        col: 1,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 1,
                        row: 1,
                        col: 2,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 2,
                        row: 1,
                        col: 3,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 3,
                        row: 2,
                        col: 1,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 4,
                        row: 2,
                        col: 2,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 5,
                        row: 2,
                        col: 3,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 6,
                        row: 3,
                        col: 1,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 7,
                        row: 3,
                        col: 2,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                    {
                        // BOARD INFO
						id: 8,
                        row: 3,
                        col: 3,
						terrain: null,
						// NPC BOARD INFO
						npcType: null,
						residentNpc: false,
						// PLAYER BOARD INFO
                        residentPlayer: false,
                    },
                ],
				terrainTypes: [
					"plains",
					"mountains",
					"forest",
					"ocean"
				],


				// PLAYERS RELATED STUFF
				// PLAYERS RELATED STUFF
				// PLAYERS RELATED STUFF
				playerPlaced: false,
				player: {
					position: {
						row: null,
						col: null
					},
					lvl: 1,
					xpPool: 0,
					hp: 10,
					ap: 1,
					ar: 1
				},


				// NPC RELATED STUFF
				// NPC RELATED STUFF
				// NPC RELATED STUFF
				npcOnMap: [],
				npcTypes: [
					{
						id: 1,
						name: "Goblin",
						hp: 2,
						ap: 1,
						ar: 1,
						xpReward: 1
					},
					{
						id: 2,
						name: "Orc",
						hp: 5,
						ap: 2,
						ar: 2,
						xpReward: 5
					},
					{
						id: 3,
						name: "Ogre",
						hp: 10,
						ap: 3,
						ar: 3,
						xpReward: 10
					},
				],
			}
		},
		// before Vue renders
		created() {
			this.socket = io("http://localhost:3000");

			window.addEventListener('keypress', this.wasdMovement);
		},
		destroyed() {
			window.removeEventListener('keypress', this.wasdMovement);
		},
		// after Vue renders
		async mounted() {
			this.gameStart()
		},
		computed: {

		},
		methods: {
			// GAME START
			gameStart() {
				// FIND A RANDOM SQUARE TO PLACE THE PLAYER INTO
				let randomPlayerSquare = Math.floor(Math.random() * 9) + 1;


				for(let i = 0; i < this.gameBoard3x3.length; i++) {
					setTimeout(() => {
						// find a random terrain type, and apply it to the current gameBoard Square
						let randomTerrainType = Math.floor(Math.random() * 4);
						this.gameBoard3x3[i].terrain = this.terrainTypes[randomTerrainType]
						console.log(this.gameBoard3x3[i].terrain, "what is the terrain")

						if(this.playerPlaced === false){
							// PLACING THE PLAYER
							// PLACING THE PLAYER
							// PLACING THE PLAYER

							// set player flag to true, tell game this has been taken care of and move on with NPC placement
							this.playerPlaced = true

							// set game board player setting to true, so that NO npcs can be placed here
							this.gameBoard3x3[randomPlayerSquare].residentPlayer = true
							
							// set this.player position information based on current gameBoard spot
							this.player.position.row = this.gameBoard3x3[randomPlayerSquare].row
							this.player.position.col = this.gameBoard3x3[randomPlayerSquare].col
							// console.log("THIS IS PLAYER POSITION", this.player.position)
						} else
						if(this.gameBoard3x3[i].residentPlayer === false) {
							// PLACING THE COMPUTER CONTROLLED CHARACTERS
							// PLACING THE COMPUTER CONTROLLED CHARACTERS
							// PLACING THE COMPUTER CONTROLLED CHARACTERS

							// find a random number between 1-3, this will correlate with the index position of the NPC type
							let randomNpcType = Math.floor(Math.random() * 3);
							console.log('Npc Choosen', randomNpcType)

							this.gameBoard3x3[i].residentNpc = true
							this.gameBoard3x3[i].npcType =  this.npcTypes[randomNpcType]
						}

					}, 1000)
				}
			},



			// wasdMovement(e) {
			// 	let cmd = String.fromCharCode(e.keyCode).toLowerCase();
			// 	console.log(cmd, "command")
			// 	console.log("W A S D Movement")

			// 	if(cmd == "d"){
			// 		this.moveRight()
			// 	}
			// 	if(cmd == "a"){
			// 		this.moveLeft()
			// 	}
			// 	if(cmd == "w"){
			// 		this.moveUp()
			// 	}
			// 	if(cmd == "s"){
			// 		this.moveDown()
			// 	}
			// },

			// CLICK MOVE
			// move(direction) {
			// 	this.socket.emit("move", direction)
			// },

			// MOVE BY ARROW KEYS
			// moveRight() {
			// 	this.socket.emit("move", "right")
			// },
			// moveLeft() {
			// 	this.socket.emit("move", "left")
			// },
			// moveUp() {
			// 	this.socket.emit("move", "up")
			// },
			// moveDown() {
			// 	this.socket.emit("move", "down")
			// },
		}
	}
</script>

<style scoped>

#control_Btn {
  box-shadow:3px 3px 5px;
}

.plainsbg {
	height: 185px;
	background: url('../assets/plains.png') no-repeat center center;
	background-size: cover;
}
.forestbg {
	height: 185px;
	background: url('../assets/forest.png') no-repeat center center;
	background-size: cover;
}
.mountainsbg {
	height: 185px;
	background: url('../assets/mountains.png') no-repeat center center;
	background-size: cover;
}
.oceanbg {
	height: 185px;
	background: url('../assets/ocean.png') no-repeat center center;
	background-size: cover;
}

</style>


