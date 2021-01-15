<template>
<div>
    <a id="show-btn" href="#" @click="showModal" ><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-arrows-move" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M7.646.146a.5.5 0 0 1 .708 0l2 2a.5.5 0 0 1-.708.708L8.5 1.707V5.5a.5.5 0 0 1-1 0V1.707L6.354 2.854a.5.5 0 1 1-.708-.708l2-2zM8 10a.5.5 0 0 1 .5.5v3.793l1.146-1.147a.5.5 0 0 1 .708.708l-2 2a.5.5 0 0 1-.708 0l-2-2a.5.5 0 0 1 .708-.708L7.5 14.293V10.5A.5.5 0 0 1 8 10zM.146 8.354a.5.5 0 0 1 0-.708l2-2a.5.5 0 1 1 .708.708L1.707 7.5H5.5a.5.5 0 0 1 0 1H1.707l1.147 1.146a.5.5 0 0 1-.708.708l-2-2zM10 8a.5.5 0 0 1 .5-.5h3.793l-1.147-1.146a.5.5 0 0 1 .708-.708l2 2a.5.5 0 0 1 0 .708l-2 2a.5.5 0 0 1-.708-.708L14.293 8.5H10.5A.5.5 0 0 1 10 8z"/>
                </svg></a>

    <b-modal ref="my-modal" hide-footer title="Move Task">
      <div class="text-center">
        <!-- <h3>Move Task</h3> -->
        <form @submit="emitToMain(task.id)">
            <select name="category" v-model="newCategory">
              <option v-for="category in categoryList" :key="category.id" :value="category.id">{{category.name}}</option>

            </select>
        </form>
      </div>
      <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close</b-button>
      <b-button type="submit" @click="emitToMain" class="mt-2" variant="outline-warning" block>Submit</b-button>
    </b-modal>
  </div>
</template>


<script>
import Swal from 'sweetalert2'

export default {
    name: "MoveCategory",
    props: ['categoryTask', 'categoryList'],
    data () {
        return {
            newCategory: ''
        }
    },
    // props: ["categoryId"],
     methods: {
      showModal() {
        if(+localStorage.getItem("id") === this.categoryTask.UserId){
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
      emitToMain (){
        const value = {
          CategoryId: this.newCategory,
          taskId: this.categoryTask.id
        }
        console.log(this.categoryTask.id, '<<<< wewwwww')
        console.log(this.newCategory, "<<< move category")
        this.$emit('moveCategory', value)
        this.hideModal()
      }
    }
}
</script>

<style>

</style>