<template>
    <div>
    <h1>todoリスト</h1>
    <!-- todo入力フォーム -->
    <form @submit.prevent="addTodo()">
        <el-input placeholder="todo" v-model="todo"></el-input>
    </form>
    <el-row :gutter="12">
        <!-- todo表示エリア -->
        <TodoItem v-for="( todo, index ) in todos" :key="index" :todo="todo" @childEmit="removeTodo(index)"/>
        <!-- issue表示エリア -->
        <IssueItem v-for="( issue, index ) in issues" :key="issue.id" :issue="issue" @childEmit="closeIssue(index)"/>
    </el-row>
    </div>
</template>
    
<script>
import axios from 'axios';
import TodoItem from '@/components/TodoItem'
import IssueItem from '@/components/IssueItem'
    
const client = axios.create({
    baseURL: `${process.env.VUE_APP_GITHUB_ENDPOINT}`,
    headers: {
        'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
        'Accept': 'application/vnd.github.v3+json',
        'Content-Type':'application/json',
    },
})
    
export default {
    // name: ['TodosIssues', 'IssueItem'],
    components: {
        TodoItem, IssueItem
    },
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