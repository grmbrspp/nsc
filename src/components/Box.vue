<template>
	<div class="col-sm-6 col-md-4 col-lg-3">
		<div class="panel panel-default">
			<div v-if="type==0" class="panel-body">
				
				<table>
					<tr><th colspan="2">If your input is base<slot></slot>:</th></tr>
					<tr>
						<td>Bin:</td>
						<td class="right">
							{{ num.toString(2) | spaces(4) }}
						</td>
					</tr>
					<tr>
						<td>Oct:</td>
						<td class="right">
							{{ num.toString(8) | spaces(4) }}
						</td>
					</tr>
					<tr>
						<td>Dec:</td>
						<td class="right">
							{{ num.toString() | spaces(3) }}
						</td>
					</tr>
					<tr>
						<td>Hex:</td>
						<td class="right">
							{{ num.toString(16) | spaces(4) }}
						</td>
					</tr>
					<span class="background">
						<slot></slot>
					</span>
				</table>
			</div>
			<div v-if="type==1" class="panel-body">
				<b>If your input is <slot></slot>:</b>
				<div 
					class="colorbox"
					:style="'background-color: #' + num.toString(16)">
					&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
				</div>
			</div>
			<div v-if="type==2" class="panel-body">
				<table>
					<tr><th colspan="2">If your input is <slot></slot>:</th></tr>
					<tr>
						<td>EU format:</td>
						<td class="right">{{ getTime(num)[0] }} UTC</td>
					</tr>
					<tr>
						<td>US format:</td>
						<td class="right">{{ getTime(num)[1] }} UTC</td>
					</tr>
				</table>
			</div>
		</div>
	</div>	
</template>

<script>
	export default {
		props: ["num", "type"],
		methods: {
			getTime(unixTS) {
				let date = new Date(unixTS*1000);
				date.setTime( date.getTime() + date.getTimezoneOffset()*60*1000 );
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				let hours = date.getHours();
				let minutes = "0" + date.getMinutes();
				let seconds = "0" + date.getSeconds();
				let usTime = month + "-" + day + "-" + year + " " + hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);
				let euTime = day + "." + month + "." + year + " " + hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);
				return [euTime, usTime];
			}
		}, 
		filters: {
			spaces (s, n) {
				let regex = new RegExp('\\B(?=(\\w{' + n + '})+(?!\\w))', "g");
				return s.replace(regex, " ");
			}
		}
	}
</script>

<style>
	b, th {
		color: #189;
	}
	table {
		width: 100%;
	}
	td {
		position: relative;
		vertical-align: top;
		z-index: 1;
	}
	td.right {
		text-align: right;
		font-weight: bold;
	}
	.colorbox {
		border: 1px solid lightgrey; 
		border-radius: 5px
	}
	.background {
		position: absolute; 
		bottom: 0px;
		right: 20px; 
		width: 100%;
		text-align: right;
		font-size: 72pt;
		font-weight: bold;
		color: #ccc;
		text-shadow: 2px 2px 2px #189;
		opacity: .2;
		user-select: none;
	}
</style>