<template>
  <div id="app">
	<Timer />
  </div>
</template>

<script>
import Timer from './components/Timer.vue'

export default {
  name: 'app',
  components: {
	Timer
  },
  mounted:function(){
	// 检查是否有权限发出通知；如果没有，则请求获得权限
	if (window.Notification && Notification.permission !== "granted") {
		Notification.requestPermission(function (status) {
			if (Notification.permission !== status) {
				Notification.permission = status;
			}
		});
	}
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
