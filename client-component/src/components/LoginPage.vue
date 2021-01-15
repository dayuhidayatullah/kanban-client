<template>
    <!-- <section class="hero is-success is-fullheight">
        <div class="hero-body">
            <div class="container has-text-centered">
                <div class="column is-4 is-offset-4">
                    <h3 class="title has-text-black">Login</h3>
                    <hr class="login-hr">
                    <p class="subtitle has-text-black">Kanban Application</p>
                    <div class="box">
                        <figure class="avatar">
                            <img src="https://www.outsystems.com/Forge_BL/rest/ComponentThumbnail/GetURL_ComponentThumbnail?ProjectImageId=25068">
                        </figure>
                        <form @submit.prevent="login">
                            <div class="field">
                                <div class="control">
                                    <input class="input is-large" v-model="email" type="email" placeholder="Your Email" autofocus="">
                                </div>
                            </div>

                            <div class="field">
                                <div class="control">
                                    <input class="input is-large" v-model="password" type="password" placeholder="Your Password">
                                </div>
                            </div>
                            <button class="button is-block is-info is-large is-fullwidth">Login <i class="fa fa-sign-in" aria-hidden="true"></i></button>
                        </form>
                        <GoogleLogin class="g-signin2 mt-3" :params="params" :onSuccess="onSuccess" :onFailure="onFailure">SIGN IN WITH GOOGLE</GoogleLogin>

                    </div>
                    <p class="has-text-grey">
                        <a href="#" @click.prevent="toRegisterPage">Register</a> &nbsp;·&nbsp;
                        <a href="#">Forgot Password</a> &nbsp;·&nbsp;
                        <a href="#">Need Help?</a>
                    </p>
                </div>
            </div>
        </div>
    </section> -->
    <section class="container" style="width: 100%" id="login-page" >
        <div class="row justify-content-center mt-5">
            <div class="col-4 mt-1">
                <div class="shadow-lg p-5 mb-5 bg-ligth rounded"> 
                    <h5 class="mb-4 text-center"><strong>KANBAN</strong></h5>
                    <form @submit.prevent="login">
                    <div class="form-group">
                        <input type="email" class="form-control rounded-pill" v-model="email" id="login-email" placeholder="Your email">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control rounded-pill" v-model="password" id="login-password" placeholder="Password">
                    </div>
                    <button type="submit" class="btn btn-primary btn-block mt-5 rounded-pill">Login</button>
                    <p class="text-muted text-center"> dont have account ? <a href="#" @click.prevent="toRegisterPage"> Register</a></p>

                    </form>
                    <GoogleLogin class="g-signin2" :params="params" :onSuccess="onSuccess" :onFailure="onFailure">SIGN IN WITH GOOGLE</GoogleLogin>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import GoogleLogin from 'vue-google-login'
export default {
    name: "LoginPage",
    props: ["registerPage"],
    components: {
        GoogleLogin
    },
    data () {
        return {
            email: '',
            password: '',
            params: {
                client_id: "830993365713-kbctbq255qvp0hm1c5gq4vp9igskv11d.apps.googleusercontent.com"
            },
            renderParams: {
                width: 250,
                height: 50,
                longtitle: true
            }
        }
    },
    methods: {
        login(){
            this.$emit("loginSubmit", this.email, this.password)
        },
        toRegisterPage(){
            this.$emit("changeRegisterPage", "register")
        },
        onSuccess(googleUser){
            console.log('masuk bosss')
            console.log(googleUser, "dari login page")
            const google_access_token =  googleUser.getAuthResponse().id_token
            this.$emit("emitGoogleLogin", google_access_token)
        },
        onFailure(){
            console.log(error)
        }
    }
}
</script>

<style>
</style>