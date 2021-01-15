<template>
    <div>
        <div class="addTask p-2 mt-1 ml-3">
                <strong>
                    <a href="#" class="nav-item text-decoration-none font-weight-bold pr-3" @click="showModal">
                <svg width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-plus text-dark" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"/>
                </svg>
                    </a>

                </strong>
                </div>
         <b-modal ref="my-modal" hide-footer title="Add Task">
      <div class="text-center">
        <!-- <h3>Move Task</h3> -->
        <form @submit.prevent="addTaskSubmit">
            <textarea v-model="title" cols="56" rows="5"></textarea>
        </form>
      </div>
      <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close</b-button>
      <b-button type="submit" @click.prevent="addTaskSubmit" class="mt-2" variant="outline-warning" block>Submit</b-button>
    </b-modal>
    </div>
    <!-- <div class="container">
        <form @submit.prevent="addTaskSubmit" :orderTask="requestTask" :category="categories">
            <textarea v-model="title" id="" cols="33" rows="5"></textarea>
            <button type="submit" class="btn btn-outline-primary bg-dark mb-2">Add</button>
        </form> 
        <button class="btn btn-outline-danger bg-dark" @click="cancelAdd">Cancel
        </button>
    </div> -->
</template>

<script>
export default {
    name: "AddTask",
    props: ["requestTask", "categories"],
    data(){
        return{
            title:'',
        }
    },
    methods: {
        showModal() {
            this.$refs['my-modal'].show()
        },
        hideModal() {
            this.$refs['my-modal'].hide()
        },
        toggleModal() {
            this.$refs['my-modal'].toggle('#toggle-btn')
        },
        addTaskSubmit(){
            this.$emit("submitAdd", {title: this.title, CategoryId: this.categories})
            this.hideModal()
        },
        cancelAdd(){
            console.log(this.requestTask)
            this.$emit("cancelAddForm", "none")
        }
    }
}
</script>

<style>

</style>