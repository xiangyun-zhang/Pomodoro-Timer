<template>
	<div class="timer">
		<div class="container-fluid">
			<p>今日你已完成 {{ haveFinished }} 个番茄时钟</p>
			<p>
				工作时长为
				<!-- <button v-on:click="function(){workTime>5?workTime-=5:workTime=5}">-</button> -->
				{{ getMinutes(workTime * 60) }}
				<!-- <button v-on:click="function(){workTime<55?workTime+=5:workTime=55}">+</button> -->
				分钟
			</p>
			<span>
				距离{{ action ? (status == 'work' ? '工作' : '休息') : '工作' }}结束还有 {{ action ? minutes : getMinutes(workTime * 60) }} 分
				{{ action ? seconds : getSeconds(workTime * 60) }} 秒
			</span>
		</div>
		<div class="container-fluid">
			<div class="row justify-content-center">
				<div class="clock mt-4" v-on:click="action ? resetTimer() : beginWork()">
					<div class="clock_timer" v-if="status == 'work' || status == 'unhold'">
						<div class="dial">
							<b>0</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>5</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>10</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>15</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>20</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>25</b>
							<br />
							<span></span>
						</div>
					</div>
					<div class="clock_timer" v-else-if="status == 'rest' && this.workCount % this.groupCount !== 0">
						<div class="dial">
							<b>0</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>5</b>
							<br />
							<span></span>
						</div>
					</div>
					<div class="clock_timer" v-else-if="status == 'rest' && this.workCount % this.groupCount == 0">
						<div class="dial">
							<b>0</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>5</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>10</b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b></b>
							<br />
							<span></span>
						</div>
						<div class="dial">
							<b>15</b>
							<br />
							<span></span>
						</div>
					</div>
					<div class="clock_dial">
						<div class="dial">
							<span></span>
							<br />
							<b>12</b>
						</div>
						<div class="dial">
							<span></span>
							<br />
							<b>3</b>
						</div>
						<div class="dial">
							<span></span>
							<br />
							<b>6</b>
						</div>
						<div class="dial">
							<span></span>
							<br />
							<b>9</b>
						</div>
					</div>
					<div class="H"><span></span></div>
					<div class="M"><span></span></div>
					<div class="S"><span></span></div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import $ from 'jquery';
var storage = window.localStorage;
export default {
	name: 'Timer',
	props: {},
	created: function() {
		// 在渲染前拿到当天已完成的次数，若没有则新建当天记数字段
		let nowTime = new Date();
		let today = nowTime.getFullYear() + '-' + (nowTime.getMonth() + 1) + '-' + nowTime.getDate();
		let finishedHash = storage.getItem('have_finished');
		if (finishedHash == null) {
			finishedHash = new Object();
			finishedHash[today] = this.haveFinished;
			storage.setItem('have_finished', JSON.stringify(finishedHash));
		} else {
			finishedHash = JSON.parse(finishedHash);
			if (finishedHash[today] == undefined) {
				finishedHash[today] = this.haveFinished;
				storage.setItem('have_finished', JSON.stringify(finishedHash));
			} else {
				this.haveFinished = finishedHash[today];
			}
		}
	},
	mounted: function() {
		// 制作表盘
		let clock_dial = document.querySelectorAll('.clock_dial .dial');
		for (let i = 0; i < clock_dial.length; i++) {
			let angle = (360 / 4) * i;
			clock_dial[i].style.transform = 'rotate(' + angle + 'deg)';
			clock_dial[i].lastChild.style.transform = 'rotate(' + -angle + 'deg)';
		}
		this.setTimer();
		let that = this;
		setInterval(function() {
			that.clockMove();
		}, 1000);
	},
	computed: {
		timerStatus: function() {
			return this.status;
		}
	},
	watch: {
		timerStatus() {
			this.$nextTick(function() {
				this.setTimer();
			});
		},
		// 每次 haveFinished 变量更新的时候，更新 localStorage 内的对应数据，完成当天数据记录
		haveFinished: function() {
			let nowTime = new Date();
			let today = nowTime.getFullYear() + '-' + (nowTime.getMonth() + 1) + '-' + nowTime.getDate();
			let finishedHash = storage.getItem('have_finished');
			finishedHash = JSON.parse(finishedHash);
			finishedHash[today] = this.haveFinished;
			storage.setItem('have_finished', JSON.stringify(finishedHash));
		}
	},
	data() {
		return {
			action: false, // 番茄时钟是否进行状态
			status: 'unhold', // 工作状态：work、rest、unhold
			groupCount: process.env.VUE_APP_SECRET, // 一个工作组包含的番茄时钟数
			workCount: 0, // 正在进行的番茄时钟完成数
			haveFinished: 0, // 已经完成的番茄时钟个数
			workTime: process.env.VUE_APP_WORK_TIME, // 番茄时钟工作时间（分）
			shortRest: process.env.VUE_APP_SHORT_TIME, // 短休息时长（分）
			longRest: process.env.VUE_APP_LONG_REST, // 长休息时长（分）
			maxtime: 0, // 倒计时总时长（秒）
			minutes: '00',
			seconds: '00',
			timer: 0 // 执行计时器
		};
	},
	methods: {
		// 获取倒计时时剩余分钟数
		getMinutes: function(seconds) {
			return Math.floor(seconds / 60) < 10 ? '0' + Math.floor(seconds / 60) : Math.floor(seconds / 60);
		},

		// 获取倒计时时剩余秒数
		getSeconds: function(seconds) {
			return Math.floor(seconds % 60) < 10 ? '0' + Math.floor(seconds % 60) : Math.floor(seconds % 60);
		},

		// 重置计时器
		resetTimer: function() {
			// 将 timer 清空，不再执行倒计时
			clearInterval(this.timer);
			this.action = false;
			this.status = 'unhold';
			this.workCount = 0;
			// 获取初始状态下需要显示的分钟数和秒数
			this.minutes = this.getMinutes(this.workTime * 60);
			this.seconds = this.getSeconds(this.workTime * 60);

			// 重置计时器表盘
			let clock_timer = document.querySelector('.clock_timer');
			clock_timer.style.transform = 'rotate(' + 0 + 'deg)';
		},

		// 倒计时逻辑
		countDown: function() {
			if (this.maxtime > 0) {
				this.minutes = this.getMinutes(this.maxtime);
				this.seconds = this.getSeconds(this.maxtime);
				--this.maxtime;
			} else {
				if (this.action && this.status == 'work') {
					this.haveFinished++;
					this.workCount++;
					if (window.Notification && Notification.permission === 'granted') {
						if (this.workCount % this.groupCount == 0) {
							let n = new Notification('时间到', { body: '这次可以休息久一些，活动下身子吧。' }); // 显示通知
						} else {
							let n = new Notification('时间到', { body: '稍微休息下，下一轮番茄时钟很快就到。' }); // 显示通知
						}
					} else {
						if (this.workCount % this.groupCount == 0) {
							alert('这次可以休息久一些，活动下身子吧。');
						} else {
							alert('稍微休息下，下一轮番茄时钟很快就到。');
						}
					}
					this.beginRest();
				} else {
					if (window.Notification && Notification.permission === 'granted') {
						let n = new Notification('休息结束', { body: '准备开始新一轮番茄时钟吧。' }); // 显示通知
					} else {
						alert('休息结束，准备开始新一轮番茄时钟吧。');
					}
					this.beginWork();
				}
			}
		},

		// 开始番茄时钟
		beginWork: function() {
			this.action = true;
			this.status = 'work';
			// 执行倒计时的时候已经延迟 一秒执行，所以总时间要先减去一秒
			this.maxtime = this.workTime * 60 - 1;
			this.timerCalibration();
			// 执行倒计时
			let that = this;
			clearInterval(this.timer);
			this.timer = setInterval(function() {
				that.countDown();
			}, 1000);
		},
		// 休息
		beginRest: function() {
			this.status = 'rest';
			let restTime = this.workCount % this.groupCount == 0 ? this.longRest : this.shortRest;
			this.maxtime = restTime * 60 - 1;
			this.timerCalibration();
			// 执行倒计时
			let that = this;
			clearInterval(this.timer);
			this.timer = setInterval(function() {
				that.countDown();
			}, 1000);
		},
		//时钟时分秒针转动
		clockMove: function() {
			let H_clock = document.querySelector('.H');
			let M_clock = document.querySelector('.M');
			let S_clock = document.querySelector('.S');

			let nowTime = new Date();
			let H = nowTime.getHours();
			let M = nowTime.getMinutes();
			let S = nowTime.getSeconds();
			//1s是6度，1m是6度，1h是30度；
			S_clock.style.transform = 'rotate(' + S * 6 + 'deg)';
			H_clock.style.transform = 'rotate(' + (H * 30 + (30 / 60) * M) + 'deg)';
			M_clock.style.transform = 'rotate(' + M * 6 + 'deg)';
		},
		// 制作计时器
		setTimer: function() {
			let timer_dial = document.querySelectorAll('.clock_timer .dial');
			for (let i = 0; i < timer_dial.length; i++) {
				let angle = (360 / 60) * i;
				timer_dial[i].style.transform = 'rotate(' + angle + 'deg)';
				timer_dial[i].firstChild.style.transform = 'rotate(' + -angle + 'deg)';
			}
		},
		// 计时器校准，0刻度对准当前分针
		timerCalibration: function() {
			// 获取当前时间
			let nowTime = new Date();
			// 获得当前分钟
			let M = nowTime.getMinutes();
			let clock_timer = document.querySelector('.clock_timer');
			// 将计时器刻度与当前分钟对齐
			clock_timer.style.transform = 'rotate(' + M * 6 + 'deg)';
		}
	}
};
</script>

<style scoped lang="scss">
.clock {
	width: 300px;
	height: 300px;
	border: 0;
	border-radius: 50%;
	position: relative;

	.dial {
		position: absolute;
		width: 20px;
		height: 100%;
		top: 0;
		left: 140px;

		span {
			width: 5px;
			height: 10px;
			background: black;
			display: inline-block;
			vertical-align: top;
		}
	}
	.clock_timer {
		font-size: 18px;
		width: 100%;
		height: 100%;
		background-color: rgb(254, 67, 101);
		border-radius: 50%;
		top: 0;
		left: 0;
		position: absolute;
		transition: 1.5s;
		.dial {
			span {
				width: 1px;
				height: 30px;
				vertical-align: bottom;
			}
		}
	}
	.clock_dial {
		font-size: 18px;
		width: 74%;
		height: 74%;
		background-color: rgb(249, 205, 173);
		top: 13%;
		left: 13%;
		border-radius: 50%;
		position: relative;
		.dial {
			left: 101px;
			span {
				height: 15px;
				display: inline-block;
			}
			b {
				display: inline-block;
			}
		}
	}

	.H,
	.M,
	.S {
		position: absolute;
		height: 100%;
		top: 0;
		left: 50%;
		-webkit-transform: translate3d(-50%, 0, 0);
		-ms-transform: translate3d(-50%, 0, 0);
		-o-transform: translate3d(-50%, 0, 0);
		text-align: center;
	}

	.H {
		span {
			margin-top: 80px;
			width: 6px;
			height: 80px;
			background: black;
			display: inline-block;
		}
	}

	.M {
		span {
			margin-top: 60px;
			height: 100px;
			width: 6px;
			background: black;
			display: inline-block;
		}
	}

	.S {
		span {
			margin-top: 45px;
			height: 120px;
			width: 3px;
			background: red;
			display: inline-block;
			position: relative;
		}

		span:after {
			content: '';
			width: 10px;
			height: 10px;
			border-radius: 50%;
			background: red;
			position: absolute;
			bottom: 10px;
			left: -3.5px;
		}
	}
}

// pad & pc
@media (min-width: 768px) {
	.clock {
		width: 500px;
		height: 500px;

		.dial {
			left: 240px;
		}

		.clock_timer {
			font-size: 25px;
			.dial {
				span {
					width: 2px;
					vertical-align: bottom;
				}
			}
		}

		.clock_dial {
			font-size: 25px;
			.dial {
				left: 175px;
				span {
					height: 20px;
				}
			}
		}
		.H {
			span {
				height: 135px;
				margin-top: 125px;
			}
		}

		.M {
			span {
				height: 165px;
				margin-top: 95px;
			}
		}
		.S {
			span {
				height: 190px;
				margin-top: 76px;
			}
		}
	}
}
</style>
