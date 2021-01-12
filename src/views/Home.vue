<template>
  <div class="home">
    <h1>ToDoリスト</h1>
    <div>
      <input type="radio" :value=1 v-model.number="dispStatus">
      <label>全て</label>
      <input type="radio" :value=2 v-model.number="dispStatus">
      <label>作業中</label>
      <input type="radio" :value=3 v-model.number="dispStatus">
      <label>完了</label>
    </div>
    <div>
      <table>
        <thead>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </thead>
        <tbody>
          <tr v-for="task in tasksFilter" v-bind:key="task.id">
            <td>{{ task.id }}</td>
            <td>{{ task.comment }}</td>
            <td><button @click="changeStatus(task.id)">{{ task.status === status.working ? '作業中' : '完了'}}</button></td>
            <td><button @click="deleteTask(task.id)">削除</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <h1>新規タスクの追加</h1>
    <div>
      <input type="text" v-model="inputTask">
      <input type="button" value="追加" @click="addTask">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data: () => {
    return {
      dispStatus: 1,
      inputTask: '',
      tasks: [],
      status: {
        all: 1,
        working: 2,
        complete: 3,
      }
    };
  },
  computed: {
    tasksFilter() {
      return this.dispStatus === this.status.all ? this.tasks : this.tasks.filter(e => e.status === this.dispStatus);
    }
  },
  methods: {
    // タスクを追加
    addTask() {
      this.tasks.push({id: this.tasks.length, comment: this.inputTask, status: this.status.working});
      this.inputTask = '';
    },
    // 状態変更
    changeStatus(taskId) {
      this.tasks[taskId].status = this.tasks[taskId].status === this.status.working ? this.status.complete : this.status.working;
    },
    // タスクを削除
    deleteTask(taskId) {
      this.tasks.splice(taskId, 1); 
      // IDを振り直す
      this.tasks.forEach((e, index) => {
        e.id = index;
      });
    },
  }
}
</script>
