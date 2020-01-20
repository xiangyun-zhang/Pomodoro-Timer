<template>
	<div class="timer">
		<p>工作时长为
		<button v-on:click="function(){workTime>5?workTime-=5:workTime=5}">-</button>
		{{ getMinutes(workTime*60) }} 
		<button v-on:click="function(){workTime<55?workTime+=5:workTime=55}">+</button>
		</p>
		<span>距离{{ action?(status=='work'?'工作':'休息'):'工作' }}结束还有 {{ action?minutes:getMinutes(workTime*60) }} 分 {{action?seconds:getSeconds(workTime*60)}} 秒</span>
		<button v-if="action" v-on:click="resetTimer()">停止</button>
		<button v-else v-on:click="beginWork()">开始</button>
	</div>
</template>

<script>
export default {
	name: 'Timer',
	props: {
	},
	data() {
		return {
			action: false,			// 番茄时钟是否进行状态
			status: 'pending',		// 工作状态：work、rest、unhold
			groupCount: 4,			// 一个工作组包含的番茄时钟数
			workCount: 0,			// 正在进行的番茄时钟完成数
			haveFinished: 0,		// 已经完成的番茄时钟个数
			workTime: 25,			// 番茄时钟工作时间（分）
			shortRest: 5,			// 短休息时长（分）
			longRest: 15,			// 长休息时长（分）
			maxtime: 0,				// 倒计时总时长（秒）
			minutes: '00',
			seconds: '00',
			timer: 0,
		}
	},
	methods:{		
		// 获取倒计时时剩余分钟数
		getMinutes: function(seconds) {
			return Math.floor(seconds / 60)<10?'0'+Math.floor(seconds / 60):Math.floor(seconds / 60);
		},
		
		// 获取倒计时时剩余秒数
		getSeconds: function(seconds) {
			return Math.floor(seconds % 60) < 10? '0'+Math.floor(seconds % 60):Math.floor(seconds % 60);
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
		},
		
		// 倒计时逻辑
		countDown: function() {
			if (this.maxtime > 0) {
				this.minutes = this.getMinutes(this.maxtime);
				this.seconds = this.getSeconds(this.maxtime);
				--this.maxtime;
			} else{
				if(this.action && this.status == 'work'){
					this.haveFinished++;
					this.workCount++;
					alert("时间到，结束！可以开始休息了！");
					this.beginRest();
				}else{
					alert("休息结束，准备开始新一轮番茄时钟吧！");
					this.beginWork();
				}
			
			}
		},
		
		// 开始番茄时钟
		beginWork: function() {
			this.action = true;
			this.status = 'work';
			// 执行倒计时的时候已经延迟 一秒执行，所以总时间要先减去一秒
			this.maxtime = this.workTime * 60-1;
			// 执行倒计时
			let that = this;
			clearInterval(this.timer);
			this.timer = setInterval(function(){that.countDown();}, 1000); 			
		},
		
		// 休息
		beginRest: function() {
			this.status = 'rest';
			let restTime = this.workCount%this.groupCount == 0? this.longRest:this.shortRest;
			this.maxtime = restTime * 60-1;
			// 执行倒计时
			let that = this;
			clearInterval(this.timer);
			this.timer = setInterval(function(){that.countDown();}, 1000); 
		}
	}
}
</script>

<style scoped>
</style>
