<template>
	<div class="todo">
		<div class="container">
			<div class="row todo-panel justify-content-end">
				<!-- Button trigger modal -->
				<div class="btn-group">
					<button class="btn btn-outline-primary btn-sm" type="button" data-toggle="modal" data-target="#addToDo">添加任务</button>
					<button class="btn btn-outline-primary btn-sm" type="button" data-toggle="modal" data-target="#toDoList">今日待办</button>
					<button class="btn btn-outline-primary btn-sm" type="button" data-toggle="modal" data-target="#toDoHistory">历史待办</button>
				</div>
			</div>
			<!-- Modal -->
			<div class="modal fade" id="addToDo" tabindex="-1" role="dialog" aria-labelledby="addToDoModalLabel" aria-hidden="true">
				<div class="modal-dialog modal-lg modal-dialog-centered" role="document">
					<div class="modal-content">
						<div class="modal-header"><h5 class="modal-title" id="addToDoModalLabel">增加任务</h5></div>
						<div class="modal-body">
							<div class="container">
								<form>
									<div class="form-group">
										<label for="taskType">任务类型</label>
										<select class="form-control" id="taskType">
											<option value="1">计划任务</option>
											<option value="2">计划外/紧急任务</option>
										</select>
									</div>
									<div class="form-group">
										<label for="taskContent">任务内容</label>
										<textarea class="form-control" id="taskContent" rows="3"></textarea>
									</div>
									<div class="form-group">
										<label for="taskEstimate">用时估计</label>
										<div class="form-row">
											<div class="col"><span>预计花费</span></div>
											<div class="col-4"><input type="number" class="form-control" id="taskEstimate" /></div>
											<div class="col"><span>个番茄时钟</span></div>
										</div>
									</div>
								</form>
							</div>
						</div>
						<div class="modal-footer">
							<button type="button" class="btn btn-secondary" data-dismiss="modal" v-on:click="resetForm()">关闭</button>
							<button type="button" class="btn btn-primary" v-on:click="checkForm()">添加任务</button>
						</div>
					</div>
				</div>
			</div>
			<div class="modal fade" id="toDoList" tabindex="-1" role="dialog" aria-labelledby="toDoListModalLabel" aria-hidden="true">
				<div class="modal-dialog modal-lg modal-dialog-centered" role="document">
					<div class="modal-content">
						<div class="modal-header"><h5 class="modal-title" id="toDoListModalLabel">今日待办任务</h5></div>
						<div class="modal-body">
							<table class="table">
								<thead class="thead-dark">
									<tr>
										<th scope="col">类型</th>
										<th scope="col">内容</th>
										<th scope="col">预计耗时</th>
										<th scope="col">实际耗时</th>
										<th scope="col"></th>
									</tr>
								</thead>
								<tbody>
									<template v-for="(todo, index) in toDoList">
										<tr :key="index">
											<td>{{ todo.type == 1 ? '计划任务' : '计划外/紧急任务' }}</td>
											<td>{{ todo.content }}</td>
											<td>{{ todo.estimate }}</td>
											<td></td>
											<td><span>&times;</span></td>
										</tr>
									</template>
								</tbody>
							</table>
						</div>
						<div class="modal-footer"><button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button></div>
					</div>
				</div>
			</div>

			<div class="modal fade" id="toDoHistory" tabindex="-1" role="dialog" aria-labelledby="toDoHistoryModalLabel" aria-hidden="true">
				<div class="modal-dialog modal-lg modal-dialog-centered" role="document">
					<div class="modal-content">
						<div class="modal-header"><h5 class="modal-title" id="toDoHistoryModalLabel">历史待办任务</h5></div>
						<div class="modal-body">
							<datepicker placeholder="选择日期" v-model="datePicked" format="yyyy-M-dd"></datepicker>
							<table class="table">
								<thead class="thead-dark">
									<tr>
										<th scope="col">类型</th>
										<th scope="col">内容</th>
										<th scope="col">预计耗时</th>
										<th scope="col">实际耗时</th>
										<th scope="col"></th>
									</tr>
								</thead>
								<tbody v-if="toDoHistory[selectDate] != undefined">
									<template v-for="(todo, id) in toDoHistory[selectDate]">
										<tr :key="id">
											<td>{{ todo.type == 1 ? '计划任务' : '计划外/紧急任务' }}</td>
											<td>{{ todo.content }}</td>
											<td>{{ todo.estimate }}</td>
											<td></td>
											<td><span>&times;</span></td>
										</tr>
									</template>
								</tbody>
								<tbody v-else><tr>(无)</tr></tbody>
							</table>
						</div>
						<div class="modal-footer"><button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button></div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import Datepicker from 'vuejs-datepicker';
import $ from 'jquery';
var storage = window.localStorage;
var nowTime = new Date();
var today = nowTime.getFullYear() + '-' + (nowTime.getMonth() + 1) + '-' + nowTime.getDate();
export default {
	name: 'ToDo',
	components: {
		Datepicker
	},
	data() {
		return {
			toDoList: {},
			toDoHistory: {},
			datePicked: '',
			selectDate: '' // 历史任务中所选的日期
		};
	},
	watch: {
		datePicked() {
			let time = new Date(this.datePicked);
			this.selectDate = time.getFullYear() + '-' + (time.getMonth() + 1) + '-' + time.getDate();
		}
	},
	created() {
		this.toDoList = this.getTasks(today) == undefined ? {} : this.getTasks(today);
		this.toDoHistory = this.getTasks() == undefined ? {} : this.getTasks();
	},
	mounted() {},
	methods: {
		// 去除字符串两端空格
		trim(str) {
			return str.replace(/(^\s*)|(\s*$)/g, '');
		},
		// 检查任务提交表格填写情况
		checkForm() {
			let content = this.trim($('#taskContent').val());
			let estimate = this.trim($('#taskEstimate').val());
			$('#taskContent').removeClass('is-invalid');
			$('#taskEstimate').removeClass('is-invalid');
			if (content == '' || estimate == '' || estimate == 0) {
				if (content == '') {
					$('#taskContent').addClass('is-invalid');
				}
				if (estimate == '' || estimate == 0) {
					$('#taskEstimate').addClass('is-invalid');
				}
			} else {
				// 当前任务
				let task = new Object();
				task['type'] = $('#taskType').val();
				task['content'] = content;
				task['estimate'] = estimate;
				task['delete'] = 0;
				this.saveTask(task);
			}
		},
		// 获取任务
		getTasks(date = '') {
			let tasks = storage.getItem('tasks');

			// 若没有，
			if (tasks != null) {
				tasks = JSON.parse(tasks);

				if (date != '') {
					tasks = tasks[date];
				}
			}
			return tasks;
		},
		// 存储任务
		saveTask(task) {
			// 获取所有任务
			let tasks = storage.getItem('tasks');
			let taskId = 0;
			// 初次使用时新建任务字段，并初始化当天对象
			if (tasks == null) {
				tasks = new Object();
				tasks[today] = new Object();
				taskId = 1;
			} else {
				tasks = JSON.parse(tasks);

				// 若当天无内容则初始化当天对象
				if (tasks[today] == undefined) {
					tasks[today] = new Object();
					taskId = 1;
				} else {
					// 有内容则taskId 递增
					taskId = Object.keys(tasks[today]).length + 1;
				}
			}
			tasks[today][taskId] = task;
			// 更新数据
			this.toDoList[taskId] = task;
			this.toDoHistory[today] = tasks[today];
			// 存入localstorage
			storage.setItem('tasks', JSON.stringify(tasks));

			this.resetForm();
			this.$forceUpdate();
		},
		//初始化表格
		resetForm() {
			$('#taskType').val(1);
			$('#taskContent').val('');
			$('#taskContent').removeClass('is-invalid');
			$('#taskEstimate').val('');
			$('#taskEstimate').removeClass('is-invalid');
		}
	}
};
</script>

<style scoped lang="scss">
.todo {
	.modal-content {
		height: 500px;
	}
}
</style>
