<template>
    <div>
        <Navbar 
            @logoutButton="logout"
        ></Navbar>
        <MainListTask 
            @dragDrop="dragDrop"
            :requestTask="orderTask"
            :categoriesList="categories"
            @emitToMainPage="emitToApp"
            @emitEditValue="editValue"
            @emitValueAdd="addValue"
            @emitDeleteMain="emitDeleteApp"
            @moveCategory="emitMoveCategory"
        >
        </MainListTask>
           

    </div>
</template>

<script>
import Navbar from "./Navbar"
import MainListTask from "./MainListTask"
export default {
    name: "MainPage",
    props: ["categories", "orderTask"],
    data(){
        return{
            id: 0
        }
    },
    components: {
        Navbar,
        MainListTask
    }, 
    methods: {
        emitDeleteApp (key) {
            this.$emit('emitDeleteToApp', key)
        },
        emitToApp(key){
            this.id = key
            console.log(key, "<-- diMain PAGEW")
            this.$emit("getTaskById", this.id)
        },
        editValue(value){
            this.$emit("emitEditValue", value)
        },
        logout(pageName){
            this.$emit("buttonLogout", pageName)
        },
        addValue(payload){
            this.$emit("emitValueAdd", payload)
        },
        emitMoveCategory (value) {
            this.$emit("moveCategory", value)
        },
        dragDrop(payload){
            this.$emit('dragDrop', payload)
        }
    }
}
</script>

<style>

</style>