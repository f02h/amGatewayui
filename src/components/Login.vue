<template>
    <form class="form form--login" @submit.prevent="login">
        <h2 class="form__title">Dashboard login</h2>

        <div class="info info--error" v-if="infoError">Login failed. Please try again.</div>

        <div :class="{'is-waiting': loader}">
            <div class="form-block">
                <input v-model.trim="username" class="field form-control" name="username"  placeholder="username"
                       required>
            </div>

            <div class="form-block">
                <input v-model.trim="password" class="field form-control" name="password" type="password" placeholder="Password"
                       required>
            </div>

            <div class="form-block form__actions">
                <router-link to="/password-reset">Lost your password?</router-link>
                <button class="button button--green form__submit">Login</button>
            </div>
        </div>
    </form>
</template>

<script>
    import VueResource from "vue-resource"
    import Vue from 'vue';
    import router from '@/router'
    import store from '@/store'
    export default {
        name: 'login',
        data () {
            return {
                loader: false,
                infoError: false,
                email: '',
                password: ''
            }
        },
        beforeCreate () {
            if (store.state.isLogged) {
                router.push('/')
            }
        },
        methods: {
            login () {
                Vue.use(VueResource);
                this.loader = true
                this.infoError = false
                this.$http.post(process.env.API_URL+'/auth/login', {
                    username: this.username,
                    password: this.password
                }).then((response) => {
                    localStorage.setItem('token', response.body.token)
                    store.commit('LOGIN_USER')
                    router.push('/')
                }, () => {
                    this.infoError = true
                    this.loader = false
                    this.password = ''
                })
            }
        }
    }
</script>
