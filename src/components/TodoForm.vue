<template lang="ko">
    <form  class='form' @submit.prevent='onSubmit'>
        <div class='flex'>
            <div class='flex-grow'>
                <input class='from-control' type='text' placeholder='오늘 할일' v-model='todo' />
            </div>
            <div>
                <button class='btn' type='submit'>추가</button>
            </div>       
        </div>     
        <div style='color:green; fontSize:12px; marginTop:5px' v-show='hasError' >
            내용이 비어있습니다
        </div>
    </form>
</template>
<script>
import {ref} from 'vue'
export default {
    emits:['add-todo'],
    setup(props,{emit}){   //context 자식->부모로 보내기
        const todo= ref('');
        const hasError=ref(false);
        const onSubmit=()=>{

            if(todo.value===''){   //todo가 빈값이면,
                hasError.value=true;
            }
            else{
                emit('add-todo', {
                id:new Date(),
                subject:todo.value,
                completed:false,
                })
                hasError.value=false;
                todo.value=''
            }
        } 

        return{
            todo,
            hasError,
            onSubmit
        }
    }
}
</script>
<style lang="scss">
    
</style>