<template>
    <div>
        <LoginPage 
            v-if="currentPage === 'Login Page'"
            @loginSubmit="login" 
            @emitGoogleLogin="googleLogin"
            @changeRegisterPage="renderRegis"
        >
        </LoginPage>
        <RegisterPage 
            v-else-if="currentPage === 'Register Page'" 
            @registerSubmit="register"
            @toLoginPage="renderLogin"
        >
        </RegisterPage>
        <MainPage 
            v-else 
            
            :categories="categoryTask"
            :orderTask="requestTaskId"
            @getTaskById="getTaskId"
            @emitEditValue="editValue"
            @buttonLogout="logout"
            @emitValueAdd="addValue"
            @emitDeleteToApp="deleteTodo"
            @moveCategory="moveCategory"
            @emitGoogleLogin="googleLogin"
            @dragDrop="dragDrop"
        >
        </MainPage>
    </div>
</template>

<script>
import LoginPage from "./components/LoginPage.vue"
import RegisterPage from "./components/RegisterPage"
import axios from "axios"
import MainPage from "./components/MainPage"
import draggable from "vuedraggable"
import Swal from 'sweetalert2'

export default {
    name:  "App",
    data(){
        return {
            name: "Dayu",
            currentPage: "Login Page",
            url: "https://kanban-app-florence.herokuapp.com",
            categoryTask: [],
            requestTaskId: []
        }
    },
    components: {
        LoginPage,
        MainPage, 
        RegisterPage,
        draggable,
        Swal
    },
    methods: {
        logout(pageName){
            Swal.fire({
                icon: 'success',
                title: 'Logout Succesfully',
                showConfirmButton: false,
                timer: 1500
            })
            localStorage.clear()
            this.currentPage = pageName
            var auth2 = gapi.auth2.getAuthInstance();
                auth2.signOut().then(function () {
                console.log('User signed out.');
            });
        },
        googleLogin(token){
            axios({
                url: this.url + "/googleLogin",
                method: "post",
                data: {
                    token
                }

            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Login Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    console.log(response, "<------- ini dari appp")
                    localStorage.setItem("access_token", response.data.access_token)
                    localStorage.setItem("id", response.data.user.id)
                    console.log(response, '<<<<<<<< app vue login   ')
                    this.currentPage = "Main Page"
                    this.fetchCategories()

                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        login(email, password){
            console.log('masuk login')
            axios({
                url: this.url + "/login", 
                method: "post",
                data: {
                    email,
                    password
                }
            })
                .then(response => {
                    localStorage.setItem("access_token", response.data.access_token)
                    localStorage.setItem("id", response.data.id)

                    console.log(response.data)
                    this.currentPage = "Main Page"
                    this.fetchCategories()
                    Swal.fire({
                        position: 'center',
                        icon: 'success',
                        title: 'Login Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })


                })
                .catch(error => {
                    console.log(error.response)
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        register(email, password){
            console.log(email, password)
            axios({
                url: this.url + "/register", 
                method: "post",
                data: {
                    email,
                    password
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Register Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    console.log(response.data)
                    this.currentPage = "Login Page"
                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.message}`,
                    })
                    console.log("masuk error bos")
                    console.log(error)
                })

        },
        renderRegis(){
            this.currentPage = "Register Page"
        },
        renderLogin(){
            this.currentPage = "Login Page"
        },
        fetchCategories(){
            axios({
                url: this.url + "/categories",
                method: "get",
                headers: {
                    access_token: localStorage.getItem("access_token")
                }
            })
                .then(response => {
                    const sortCategories = response.data.sort((a, b) => a.id-b.id)
                    this.categoryTask = sortCategories
                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        getTaskId(key){
            axios({
                url: this.url + `/tasks/${key}`,
                method: "get",
                headers: {
                    access_token: localStorage.getItem("access_token")
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    this.requestTaskId = response.data
                    console.log(response.data)
                })
                .catch(error =>  {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        editValue(value){
            console.log(value, "dari APPPPPP")
            const title = value.title
            const CategoryId = value.CategoryId
            const UserId = value.UserId
    
            axios({
                url: this.url + "/tasks/" + value.id,
                method: "put",
                headers: {
                    access_token: localStorage.getItem('access_token')
                },
                data: {
                    title,
                    CategoryId,
                    UserId
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Edit Task Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    console.log(response.data, "<--- ini sudah berhasil diupdate ")
                    this.fetchCategories()
                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        addValue(payload){
            const title = payload.title
            const CategoryId = payload.CategoryId
            axios({
                url: this.url + "/tasks",
                method: "post",
                headers: {
                    access_token: localStorage.getItem("access_token")
                },
                data: {
                    title,
                    CategoryId
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Add Task Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    console.log("berhasil")
                    this.fetchCategories()
                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        deleteTodo (key) {
            const id = +localStorage.getItem("id")
            const swalWithBootstrapButtons = Swal.mixin({
            customClass: {
                confirmButton: 'btn btn-success',
                cancelButton: 'btn btn-danger mr-3'
            },
                buttonsStyling: false
            })
            if(key.UserId === id){

            swalWithBootstrapButtons.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonText: 'Yes, delete it!',
            cancelButtonText: 'No, cancel!',
            reverseButtons: true
            }).then((result) => {
            if (result.isConfirmed) {
                axios({
                url: this.url + '/tasks/' + key.id,
                method: "delete",
                headers: {
                    access_token: localStorage.getItem('access_token')
                }
            })
                .then(response => {
                    swalWithBootstrapButtons.fire(
                    'Deleted!',
                    'Your Task has been deleted.',
                    'success'
                    )
                    this.fetchCategories()
                })
                .catch(error => {
                    swalWithBootstrapButtons.fire(
                    'Cancelled',
                    `${error.message}`,
                    'error'
                    )
                    console.log(error.message)
                })
                
            } else if (
                /* Read more about handling dismissals below */
                result.dismiss === Swal.DismissReason.cancel
            ) {
                swalWithBootstrapButtons.fire(
                'Cancelled',
                'Your Task is safe :)',
                'error'
                )
            }
            })
            } else {
                swalWithBootstrapButtons.fire(
                'No Authorization',
                'you are not entitled to delete this task:)',
                'error'
                )
            }
            
        },
        moveCategory (value) {
            console.log(value, "dari Appppp")
            axios({
                url: this.url + "/tasks/" + value.taskId,
                method: "patch",
                headers: {
                    access_token: localStorage.getItem("access_token")
                },
                data: {
                    CategoryId: value.CategoryId,
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Move Task Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    console.log("berhasil")
                    this.fetchCategories()
                })
                .catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        },
        dragDrop(payload){
            axios({
                url: this.url + "/tasks/" + payload.taskId,
                method: "patch",
                headers: {
                    access_token: localStorage.getItem("access_token")
                },
                data: {
                    CategoryId: payload.CategoryId,
                }
            })
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: 'Move Task Succesfully',
                        showConfirmButton: false,
                        timer: 1500
                    })
                    this.fetchCategories()
                })
                .catch(error => {
                    window.history.go()
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: `${error.response.data.msg}`,
                    })
                    console.log(error)
                })
        }



    },
    created(){
        if(localStorage.getItem("access_token")){
            this.currentPage = "Main Page"
            this.fetchCategories()
        }
        else{
            this.currentPage = "Login Page"
        }
    }

    
}
</script>

<style>

</style>