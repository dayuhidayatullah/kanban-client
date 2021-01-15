<template>
    <div class="board justify-content-center" >
            <header class="contianer d-flex justify-content-between ">
                <h4 class="text-decoration-underline bg-ligth mt-3 ml-3"> {{category.name}}
                </h4>
                
            <AddTask :requestTask="orderTaskId" :categories="category.id"  @submitAdd="valueAdd"  > </AddTask>
            </header>
        
        
        <div class="dropzone justify-content-center">
            <draggable :id="category.id" group="task" @start="drag=true" @end="(e) => dragAndDrop(e)">
            <div class="card" v-for="(categoryTask, index) in tasks" :key="index" :id="categoryTask.id">
                <div :id="categoryTask.id" name="tasks">

                <div class="content">{{categoryTask.title}}</div>

                <p class="text-muted m-0"><small>{{category.createdAt.substr(0, 10)}} </small></p>   
                <p class="text-muted m-0"><small>{{categoryTask.User.email.split('@')[0]}}</small></p>
                <div class="container d-flex justify-content-end">
                    <div class="deleteTask">
                        <a href="#" @click="deleteToApp(categoryTask)" class="text-decoration-none float-right" >
                            <svg width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-trash" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                                <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                                <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4L4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                            </svg>
                        </a>
                    </div>
                    <!-- <div class="moveCategory" >
                        <a href="#" class="text-decoration-none float-right pr-3 pl-3" @click="moveCategory(category.name)" data-toggle="modal" data-target="#modalMove">
                            <svg width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-arrows-move" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                                <path fill-rule="evenodd" d="M7.646.146a.5.5 0 0 1 .708 0l2 2a.5.5 0 0 1-.708.708L8.5 1.707V5.5a.5.5 0 0 1-1 0V1.707L6.354 2.854a.5.5 0 1 1-.708-.708l2-2zM8 10a.5.5 0 0 1 .5.5v3.793l1.146-1.147a.5.5 0 0 1 .708.708l-2 2a.5.5 0 0 1-.708 0l-2-2a.5.5 0 0 1 .708-.708L7.5 14.293V10.5A.5.5 0 0 1 8 10zM.146 8.354a.5.5 0 0 1 0-.708l2-2a.5.5 0 1 1 .708.708L1.707 7.5H5.5a.5.5 0 0 1 0 1H1.707l1.147 1.146a.5.5 0 0 1-.708.708l-2-2zM10 8a.5.5 0 0 1 .5-.5h3.793l-1.147-1.146a.5.5 0 0 1 .708-.708l2 2a.5.5 0 0 1 0 .708l-2 2a.5.5 0 0 1-.708-.708L14.293 8.5H10.5A.5.5 0 0 1 10 8z"/>
                            </svg>
                        </a>
                    </div> -->
        <div class="editTask pr-5">
            <EditPage :requestTask="categoryTask" @emitInEdit="valueEdit"> </EditPage>
        </div>
                <MoveCategory @moveCategory="moveCategory" id="modalMove" :categoryTask="categoryTask" :categoryList="categoryList"> </MoveCategory>   
                    

                </div>
                </div>

            
            </div>
            </draggable>
        </div>
        

        
         

    </div>
</template>
    
<script>
import EditPage from './EditPage'
import AddTask from './AddTask'
import MoveCategory from './MoveCategory' 
import draggable from 'vuedraggable'

export default {
    name: "MainBoard",
    props: ["category", "orderTaskId", "tasks", "categoryList"],
    data(){
        return {
            id: 0,
            isDisplay: 'none',
            isDisplayAdd: 'none',
            isDisplayModal: 'none',
            editValue: [],
            categoryId: 0,
        }
    },
    components: {
        EditPage,
        AddTask,
        MoveCategory,
        draggable
    },
    methods:{
        deleteToApp (payload) {
            this.$emit('emitDeleteMain', payload)
        },
        emitToMainList(key){
            console.log(key)
            this.isDisplay = "block"
            this.id = key
            this.$emit("emitMainListParent", this.id)
        },
        displayFormEdit(display){
            this.isDisplay = display
        },
        valueEdit(value){
            this.editValue = value
            this.$emit("emitEditValue", value)
        },
        addFormTask(id){
            this.categoryId = id
            this.isDisplayAdd = "block"
        },
        valueAdd(payload){
            this.$emit("emitAddValue", payload) 
        },
        cancelFormAdd(display){
            this.isDisplayAdd = display
        },
        moveCategory(category){
            this.$emit('moveCategory', category)
        },
        dragAndDrop(e){
            const payload = {
                taskId: e.item.id,
                CategoryId: e.to.id
            }
            this.$emit("dragDrop", payload)
        }
    }

}
</script>

<style>

</style> 