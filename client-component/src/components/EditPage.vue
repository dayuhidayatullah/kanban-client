<template>
    <div>
            <a href="#" class="ml-3" @click="showModal">
                <svg  width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-pencil" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5L13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175l-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                </svg> 
            </a> 
         <b-modal ref="my-modal" hide-footer title="Edit Task">
      <div class="text-center">
        <!-- <h3>Move Task</h3> -->
        <form @submit.prevent="editTask">
            <textarea v-model="title" cols="56" rows="5"></textarea>
        </form>
      </div>
      <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close</b-button>
      <b-button type="submit" @click.prevent="editTask" class="mt-2" variant="outline-warning" block>Submit</b-button>
    </b-modal>
    </div>
</template>

<script>
import Swal from 'sweetalert2'
export default {
    name: "EditPage",
    props: ["requestTask"],
    data(){
        return{
            title: this.requestTask.title
        }
    },
    methods: {
        showModal() {
            if(+localStorage.getItem("id") === this.requestTask.UserId){
                this.$refs['my-modal'].show()
            } else {
               Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: `you are not autorized to access this task`,
                })
            }
        },
        hideModal() {
            this.$refs['my-modal'].hide()
        },
        toggleModal() {
            this.$refs['my-modal'].toggle('#toggle-btn')
        },
        editTask(){
            this.$emit("emitInEdit", {title:this.title, id: this.requestTask.id, CategoryId: this.requestTask.CategoryId, UserId: this.requestTask.UserId})
            this.hideModal()
        },
        cancelEdit(){
            this.$emit("displayNone", 'none')
        }
    }
}
</script>

<style>

</style>