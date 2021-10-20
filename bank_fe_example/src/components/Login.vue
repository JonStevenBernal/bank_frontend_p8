<template>
    <div class="loginUser">   
        <div class="containerLoginUser">
            <h2>Iniciar Sesión</h2>
            <form v-on:submit.prevent="processLoginUser">
                <input type="text" placeholder="username" v-model="user.username">
                <br />
                <input type="password" placeholder="password" v-model="user.password">
                <br />
                <button type="submit">Iniciar Sesión</button>
            </form>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "LogIn",
        
        data: function(){
            return {
                user: {
                    username:"",
                    password:""
                }
            }
        },

        methods: {
            processLoginUser: function(){
                axios.post(
                    'http://localhost:8000/login/',
                    this.user,
                    {headers:{}}
                )
                .then((result) => {
                    let dataLogin = {
                        username     : this.user.username,
                        token_access : result.data.access,
                        token_refresh: result.data.refresh
                    }
                    console.log(dataLogin);
                    this.$emit('completedLogIn', dataLogin)
                })
                .catch((error) => {
                    if(error.response.status == "401")
                        alert("Las credenciales son incorrectas.");
                });
            }
        }
    }
</script>

<style>
    .loginUser{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .containerLoginUser {
        border: 3px solid #283747;
        border-radius: 10px;
        width: 25%;
        height: 60%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .loginUser h2{
        color: #283747;
    }

    .loginUser form{
        width: 70%;
    }

    .loginUser input{
        height: 40px;
        width: 100%;
        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;
        border: 1px solid #283747;
    }

    .loginUser button{
        width: 100%;
        height: 40px;
        color: #E5E7E9;
        background: #283747;
        border: 1px solid #E5E7E9;
        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0;
    }

    .loginUser button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
    }
</style>