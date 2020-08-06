<template>
	<div class="container">

		<div class="row">
			<transition 
				enter-class="fade-enter"
				enter-active-class="fade-enter-active"
				leave-active-class="fade-leave-active"
				appear>
				<div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
					<h1>Number System Converter</h1>
					<input 
						type="text" 
						v-model="rawinput" 
						placeholder="Enter a number base 2, 8, 10 or 16" 
						autofocus>
					<hr>
				</div>
			</transition>
		</div>

		<div class="row">
			<app-box
				v-for="sys in validInputSystems"
				:key="sys.id"
				:num="output(sys.base)"
				:type="sys.type"
				:len="input.length">
				{{ sys.txt }}
			</app-box>
		</div>

	</div>
</template>

<script>
	import Box from "./components/Box.vue";

	export default {
		data() {
			return {
				rawinput: "",
				inputSystems: [
					{id: 2, txt: 2, base: 2, type: 0},
					{id: 8, txt: 8, base: 8, type: 0},
					{id: 10, txt: 10, base: 10, type: 0},
					{id: 16, txt: 16, base: 16, type: 0},
					{id: "col", txt: "a color code", base: 16, type: 1},
					{id: "uts", txt: "a unix time", base: 10, type: 2},
				]
			}
		},
		computed: {
			input() {
				return this.rawinput.replace(/[^0-9a-fA-F]/g, "");
			},
			validInputSystems() {
				return this.inputSystems.filter(this.checkInput);
			}
		},
		methods: {
			checkInput(sys) {
				switch(sys.id){
					case 2:
						return this.input.match("^([0-1]{1,53})$");
					case 8:
						return this.input.match("^([0-7]{1,17})$");
					case 10:
						return this.input.match("^([0-9]{1,15})$");
					case 16:
						return this.input.match("^([0-9a-fA-F]{1,13})$");
					case "col":
						return this.input.match("^([0-9a-fA-F]{3}|[0-9a-fA-F]{6})$");
					case "uts":
						return this.input.match("^([0-9]{1,10})$");
				}
			},
			output(n) {
				return parseInt(this.input, n);
			}
		},
		components: {
			appBox: Box
		}

	}
</script>

<style>
	h1 {
		text-align: center;
	}
	input {
		width: 100%
	}

	.fade-enter{
		opacity: 0;
	}
	.fade-enter-active{
		transition: opacity 1s;
	}
	.fade-leave-active{
		transition: opacity 1s;
		opacity: 0
	}
</style>
