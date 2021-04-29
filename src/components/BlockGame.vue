<template>
	<v-main class="app" style="background:black;height:100vh;width:100vw">
    <v-row style="margin-top:40px">
      <!-- THE BLOCK INCLUDING GAME AND CONTROLS -->
      <v-col cols="5" style="background-color:maroon;border:4px ridge gold;margin-left:50px;">
        <!-- GAME CANVAS -->
        <v-row>
          <v-col>
            <canvas 
				ref="game"
				id="myCanvas"
				width="600"
				height="600"
				style="border:2px ridge silver;margin-top:20px;margin-left:25px;background-color:rgb(70, 69, 69)"
            >



				<!-- <div x=0 y=0 style="position:0,0;" :color={green}>

				</div>
				<div x=200 y=200 style="position:200,200; background-color:yellow">

				</div>
				<div x=0 y=0 style="position:0,0; background-color:green">

				</div>
				<div x=0 y=0 style="position:0,0; background-color:green">

				</div> -->

            </canvas>
          </v-col>
        </v-row>

        <!-- GAME BUTTONS -->
        <v-row>
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
        </v-row>
      </v-col>

      <v-col cols="7">
      </v-col>
    </v-row>


	</v-main>
</template>

<script>
	import io from "socket.io-client"; //eslint-disable-line vue/no-unused-components

	export default {
		name: 'BlockGame',
		components: {
		},
		data() {
			return {
				green: "green",
				socket: {},
				context: {},
				position: {
					x: 0,
					y: 0
				},
				gameBoard: [
					{
						currentX: 0,
						currentY: 0, 
						title: "square1",
						biom: null,

					},
				],
				// DISPLAY OF GRID BY PIXEL ASSOCIATION
				// X=0 Y=0 this would be top left box
				// X is horizontal
				// Y is virtical
				gridInfo: [
					[
						{
							row1box1: 'x=0,y=0'
						}, 
						{
							row1box2: 'x=200,y=0'
						}, 
						{
							row1box3: 'x=400,y=0'
						}
					],
					[
						{
							row2box1: 'x=0,y=200'
						},
						{
							row2box2: 'x=200,y=200'
						},
						{
							row2box3: 'x=400,y=200'
						}
					],
					[
						{
							row3box1: 'x=0,y=400'
						},
						{
							row3box2: 'x=200,y=400'
						},
						{
							row3box3: 'x=400,y=400'
						}
					]
				]
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
			// this.fillGrid(this.grid5x5Loop)

			this.draw()

			this.context = this.$refs.game.getContext("2d");

			await this.socket.on("position", data => {
				// const wideness = this.$refs.game.width
				// const tallness = this.$refs.game.height

				const wideness = 600
				const tallness = 600

				this.position = data;
				this.context.clearRect(0, 0, wideness, tallness);
				this.context.fillRect(this.position.x, this.position.y, 200, 200);
				console.log(this.position.x, "X")
				console.log(this.position.y, "Y")

				//BOTTOM RIGHT CORNER
				if(this.position.x === 400 && this.position.y === 400){
					let green = this.$refs.game.getContext('2d');
					green.fillStyle = 'green';
					green.fillRect(400, 400, 200, 200)
				}

				//TOP RIGHT CORNER
				if(this.position.x === 400 && this.position.y === 0){
					let yellow = this.$refs.game.getContext('2d');
					yellow.fillStyle = 'yellow';
					yellow.fillRect(0, 0, 400, 0)
				}

			});
		},
		computed: {

		},
		methods: {
			draw() {
				let c = document.getElementById("myCanvas");
				let ctx = c.getContext("2d");
				ctx.moveTo(400, 400);
				ctx.lineTo(400, 0);
				ctx.stroke();
			},
			wasdMovement(e) {
				let cmd = String.fromCharCode(e.keyCode).toLowerCase();
				console.log(cmd, "command")
				console.log("W A S D Movement")

				if(cmd == "d"){
					this.moveRight()
				}
				if(cmd == "a"){
					this.moveLeft()
				}
				if(cmd == "w"){
					this.moveUp()
				}
				if(cmd == "s"){
					this.moveDown()
				}
			},
			// GAME TIMER
			// gameTimer() {
			// },

			// CLICK MOVE
			move(direction) {
				this.socket.emit("move", direction)
			},

			// MOVE BY ARROW KEYS
			moveRight() {
				this.socket.emit("move", "right")
			},
			moveLeft() {
				this.socket.emit("move", "left")
			},
			moveUp() {
				this.socket.emit("move", "up")
			},
			moveDown() {
				this.socket.emit("move", "down")
			},




			// fillGrid(grid) {
			// 	let loopTimer = grid.length
			// 	console.log(loopTimer)
			// }
		}
	}
</script>

<style scoped>
	/* .grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
		grid-gap: 1rem;
	}

	.grid > div {
	display: grid;
	background: black;
	padding: 1px;
	width: 50px;
	height: 50px;
	}

	.grid > div::before {
	content: "";
	padding-bottom: 100%;
	display: block;
	}

	.grid > div::before, .grid > div > img {
	grid-area: 1 / 1 / 2 / 2;
	}	 */

#control_Btn {
  box-shadow:3px 3px 5px;
}

</style>
