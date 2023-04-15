<template>
  <div>
    <h1>issueリスト</h1>
    <el-button type="success" @click="getIssues()">issue取得</el-button>
    <el-row :gutter="12">
      <!-- コード1 indexも使用できるようにune addition -->
      <el-col :span="12"  v-for="( issue, index ) in issues" :key="issue.id">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
          <el-row :gutter="12">
            <el-col :span="21">{{ issue.title }}</el-col>
            <el-col :span="3">
              <el-button @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
            </el-col>
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>

import axios from 'axios';

const client = axios.create({
  baseURL: "https://api.github.com/repos/Abdoul08/todo-project",
  headers: {
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
    'Authorization': 'token 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'
  },
})

export default {
  name: 'TodosIssues',
  data () {
    return {
      todo: '',
      todos: [],
      issues: []
    }
  },
  methods: {
    // ここからtodoの管理
    addTodo(){
      this.todos.push(this.todo);
      this.todo= '';
    },
    removeTodo(index){
      this.todos.splice(index, 1);
    },
    // ここからissueの管理
    closeIssue(index){
      const target = this.issues[index];
      client.patch(`/issues/${target.number}`,
          {
            state: "closed"
          },
        )
        .then(() => {
         this.issues.splice(index, 1);
      })
    },
    getIssues() {
      client.get('issues')
        .then((res) => {
          this.issues = res.data
      })
    }
  },
  created() {
    this.getIssues();
  }
}
</script>