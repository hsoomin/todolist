<template lang="">
    <div class="wrap">
        <h1>TODO PAGE</h1>
        <div v-if="loading">
            Loading...
        </div>
        <form v-else @submit.prevent="onSave">
            <div class="row">
                <div class="form-group">
                    <label>Todo Subject</label>
                    <input type="text" v-model="todo.subject">
                </div>
                <div class="form-group">
                    <label class="display">status</label>
                    <div class="position">
                        <button @click="toggleTodoStatus" class="btn" type="button" :class="todo.completed ? 'btnG':'btnR' ">{{todo.completed ? '완료':'다시'}}</button>
                    </div>
                </div>
            </div>
    
            <button @click="onSave" class="btn" :class="[todoUpdated ? 'btnR' : '']" type="submit" :disabled="!todoUpdated">저장</button>
            <button class="btn" @click="moveToTodoListPage" >취소</button>
        </form>
    </div>
</template>
<script>
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
import {ref, computed} from 'vue'; 
import _ from 'lodash';

export default {
    setup(){
        const route=useRoute();
        const router=useRouter();
        const todo=ref(null);
        const loading=ref(true);
        const todoId=route.params.id;
        const originalTodo=ref(null);


        const getTodo=async()=>{
            const res=await axios.get(`http://localhost:3000/todos/${todoId}`);
                todo.value={...res.data};
                originalTodo.value={...res.data};
                loading.value=false;
        }
        const toggleTodoStatus =()=>{
            todo.value.completed=!todo.value.completed
        }
        const moveToTodoListPage=()=>{
            router.push({
                name:"todos"
            })
        }
        const todoUpdated =computed(()=>{
            return !_.isEqual(todo.value, originalTodo.value);
        })
        const onSave=async()=>{
            const res=await axios.put(`http://localhost:3000/todos/${todoId}`,{
                subject:todo.value.subject,
                completed:todo.value.completed
            });
            // console.log(res)
            originalTodo.value={...res.data}
        }

        getTodo();

        return{
            todo,
            loading,
            toggleTodoStatus,
            moveToTodoListPage,
            onSave,
            todoUpdated
        }
    }
    
}
</script>
<style lang="scss">
    .wrap{
        width: 500px;
        margin: 0 auto;
        h1{text-align: center;}
        >div{}
        form{
            .row{
                .form-group{
                    position: relative;
                    display: flex;
                    flex-direction: column;
                    label{margin-bottom: 10px;}
                    .display{display: none;}
                    input{
                        padding: 10px;
                        box-sizing: border-box;
                        margin-bottom: 20px;
                    }
                    .position{
                        position: absolute;
                        top: -53px;
                        right: 5px;
                    }
                }
            }
            .btn{
                border: none; 
                padding: 5px; 
            }
            .btnR{
                background: tomato;
                color: #fff;
            }
            .btnG{
                background: green; 
                color: #fff;
            } 

        }
    }
</style>