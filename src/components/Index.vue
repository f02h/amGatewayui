<template>
    <div>

        <button id="btn4" class="" v-on:click="getMsgs">getMsgs</button>
        <button id="btn" class="" v-on:click="getData">Load data</button>
        <button id="btn2" class="" v-on:click="logout">Logout</button>
        <button id="btn3" class="" v-on:click="getKey">getKey</button>

        <div v-if="loading">
            Loading.....
        </div>

        <div class="wrapper">
            <div class="row">
                <div v-for="user in data" :key="user.id">
                    <div class="col-md-4 cards">
                        <div>
                            <h3>{{ user.id }}</h3>
                            <p>{{ user.name }}</p>
                            <p>{{ user.email }}</p>
                        </div>
                    </div>
                </div>
            </div>

        </div>

        <div class="wrapper">
            <div class="row">
                <div v-for="msg in msgs" :key="msg.msgId">
                    <div class="col-md-4 cards">
                        <div>
                            <h3>{{ msg.msgId }}</h3>
                            <p>{{ msg.domain }}</p>
                            <p>{{ msg.msgAction }}</p>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

</template>

<script>
    //import axios from 'axios';
    import axios from 'axios'
    import router from '@/router'
    import store from '@/store'
    export default {
        name: 'app',

        beforeCreate () {
            if (!store.state.isLogged) {
                router.push('/login')
            }
        },
        data () {
            return {
                data: [],
                loading: false
            }
        },
        msgs () {
            return {
                msgs: [],
                loading: false
            }
        },

        methods: {
            getData: function () {
                this.loading = true;
                axios.get(process.env.API_URL+`/users?token=`+localStorage.getItem('token'))
                    .then((response)  =>  {
                        this.loading = false;
                        this.data = response.data;
                    }, (error)  =>  {
                        console.log(error);
                        this.loading = false;
                    })
            },
            logout: function () {
                router.push('/logout')
            },
            getKey: function () {
                this.loading = true;
                axios.get(process.env.API_URL+`/key?token=`+localStorage.getItem('token'))
                    .then((response)  =>  {
                        this.loading = false;
                        this.data = response.data;
                    }, (error)  =>  {
                        console.log(error);
                        this.loading = false;
                    })
            },
            getMsgs: function () {
                this.loading = true;
                axios.get(process.env.API_URL+`/msg?token=`+localStorage.getItem('token'))
                    .then((response)  =>  {
                        this.loading = false;
                        this.msgs = response.data;
                    }, (error)  =>  {
                        console.log(error);
                        this.loading = false;
                    })
            }
        },
    }

</script>