<template>
    <!-- <section class="hero is-success is-fullheight">
        <div class="hero-body">
            <div class="container has-text-centered">
                <div class="column is-4 is-offset-4">
                    <h3 class="title has-text-black">Register</h3>
                    <hr class="login-hr">
                    <p class="subtitle has-text-black">Kanban Application</p>
                    <div class="box">
                        <figure class="avatar">
                            <img src="https://www.outsystems.com/Forge_BL/rest/ComponentThumbnail/GetURL_ComponentThumbnail?ProjectImageId=25068">
                        </figure>
                        <form @submit.prevent="register">
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
                            <button class="button is-block is-info is-large is-fullwidth">Register <i class="fa fa-sign-in" aria-hidden="true"></i></button>
                        </form>

                    </div>
                    <p class="has-text-grey">
                        <a href="#" @click.prevent="backToLogin">Login</a> &nbsp;·&nbsp;
                        <a href="#">Forgot Password</a> &nbsp;·&nbsp;
                        <a href="#">Need Help?</a>
                    </p>
                </div>
            </div>
        </div>
    </section> -->
    <section class="container"  id="register-page">
        <div class="row justify-content-center mt-5 p-5">
            <div class="col-4 mt-1">
                <div class="shadow-lg p-5 mb-5 bg-white rounded">
                    <h5 class="mb-4 text-center"><strong>KANBAN</strong></h5>
                    <form @submit.prevent="register">
                    <div class="form-group">
                        <input type="email" class="form-control rounded-pill" v-model="email" id="register-email" placeholder="Your email">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control rounded-pill" v-model="password" id="register-password" placeholder="Password">
                    </div>
                    <button type="submit " class="btn btn-primary btn-block mt-5 rounded-pill">Register</button>
                    <p class="text-muted text-center mt-3"> Have Account? <a href="#" @click.prevent="backToLogin">Login </a> </p>

                    </form>

                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: 'RegisterPage',
    data(){
        return{
            email: '',
            password: ''
        }
    },
    methods: {
        register(){
            this.$emit('registerSubmit', this.email, this.password)
            this.$emit('toLoginPage')
        },
        backToLogin(){
            this.$emit('toLoginPage')
        }
    }

}
</script>

<style>

</style>