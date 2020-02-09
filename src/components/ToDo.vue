<template>
	<div class="todo">
		<div class="container">
			<div class="row todo-panel justify-content-end">
				<!-- Button trigger modal -->
				<div class="btn-group">
					<button class="btn btn-outline-primary btn-sm" type="button" data-toggle="modal" data-target="#addToDo">添加任务</button>
					<button class="btn btn-outline-primary btn-sm" type="button" data-toggle="modal" data-target="#toDoList">今日待办</button>
				</div>
			</div>
			<!-- Modal -->
			<div class="modal fade" id="addToDo" tabindex="-1" role="dialog" aria-labelledby="addToDoModalLabel" aria-hidden="true">
				<div class="modal-dialog" role="document">
					<div class="modal-content">
						<div class="modal-header">
							<h5 class="modal-title" id="addToDoModalLabel">增加任务</h5>
							<button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
						</div>
						<div class="modal-body">
							<div class="container">
								<form>
									<div class="form-group">
										<label for="taskType">任务类型</label>
										<select class="form-control" id="taskType">
											<option>计划任务</option>
											<option>计划外/紧急任务</option>
										</select>
									</div>
									<div class="form-group">
										<label for="taskDate">任务日期</label>
										
									</div>
									<div class="form-group">
										<label for="taskContent">任务内容</label>
										<textarea class="form-control" id="taskContent" rows="3"></textarea>
									</div>
									<div class="form-group">
										<label for="taskEstimate">用时估计</label>
										<div class="form-row">
											<div class="col"><span>预计花费</span></div>
											<div class="col-1"><input type="number" class="form-control" id="taskEstimate" /></div>
											<div class="col"><span>个番茄时钟</span></div>
										</div>
									</div>
								</form>
							</div>
						</div>
						<div class="modal-footer">
							<button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
							<button type="button" class="btn btn-primary">Save changes</button>
						</div>
					</div>
				</div>
			</div>
			<div class="modal fade" id="toDoList" tabindex="-1" role="dialog" aria-labelledby="toDoListModalLabel" aria-hidden="true">
				<div class="modal-dialog" role="document">
					<div class="modal-content">
						<div class="modal-header">
							<h5 class="modal-title" id="toDoListModalLabel">今日待办任务</h5>
							<button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
						</div>
						<div class="modal-body">...</div>
						<div class="modal-footer">
							<button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
							<button type="button" class="btn btn-primary">Save changes</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'ToDo'
};
</script>

<style></style>
