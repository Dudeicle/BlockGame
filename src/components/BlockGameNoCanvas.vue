<template>
	<v-container fluid style="width:600px;height:600px;">
			<v-row>
				<v-col v-for="item in gameBoard3x3" :key="item.id" cols="4"  style="border:1px ridge crimson;width:200px;height:200px;padding:1%;background-color:black;">
					<!-- Greeting: {{ item.hello }}
					<br/>
					Row: {{ item.row }}
					<br/>
					Col: {{ item.col }}
					<br />
					Player? {{ item.residentPlayer }}
					<br />
					NPC? {{ item.residentNpc }}
					<p v-if="item.residentPlayer === true"><span>THE PLAYER IS HERE</span></p> -->
					<img height="185px" style="" src="../assets/forest.png" alt="">
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
				
				for(let i = 0; i < this.gameBoard3x3.length; i++) {
					setTimeout(() => {
					console.log(i, 'This is i')

					let randomSquare = Math.floor(Math.random() * 9) + 1;
					// console.log('Square Choosen', randomSquare)

					// let randomNpc = Math.floor(Math.random() * 3) + 1;
					// console.log('Npc Choosen', randomNpc)

					// PLACING THE PLAYER
					if(this.playerPlaced === false){
						// console.log("Player Square", randomSquare)
						// console.log("Player i number", i)

						// GAME BOARD SETTINGS BEING SET
						this.gameBoard3x3[randomSquare].residentPlayer = true
						
						// PLAYER SETTINGS BEING SET
						this.player.position.row = this.gameBoard3x3[randomSquare].row
						this.player.position.col = this.gameBoard3x3[randomSquare].col
						this.playerPlaced = true
						console.log("THIS IS PLAYER POSITION", this.player.position)
					}

					// PLACING THE COMPUTER CONTROLLED CHARACTERS
					if(this.playerPlaced === true){

						// GAME BOARD SETTINGS BEING SET
						// this.gameBoard3x3[randomSquare].residentNpc = true

						// NPC SETTINGS BEING SET

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

</style>


