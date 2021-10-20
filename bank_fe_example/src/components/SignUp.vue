<template>
    <div class="signUpUser">
        <div class="containerSignUpUser">
            <h2>Registrarse</h2>
            <form v-on:submit.prevent="processSignUp">
                <input type="text" placeholder="username" v-model="user.username">
                <br />
                <input type="password" placeholder="password" v-model="user.password">
                <br />
                <input type="text" placeholder="nombre" v-model="user.name">
                <br />
                <input type="email" placeholder="email" v-model="user.email">
                <br />
                <input type="number" placeholder="Saldo Inicial" v-model="user.account.balance">
                <br />
                <button type="submit">Registrarse</button>
            </form>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name:'SignUp',

        data:function(){
            return{
                user:{
                    username:"",
                    password:"",
                    name    :"",
                    email   :"",
                    account:{
                        lastDataChange:(new Date()).toJSON.toString(),
                        balance       :0,
                        isActive      :true
                    }
                }
            }
        },

        methods:{
            processSignUp:function(){
                console.log(user);
                axios.post(
                    'http://localhost:8000/user/',
                    this.user,
                    {headers:{}}
                )
                .then((result) => {
                    let dataSignUp = {
                        username     : this.user.username,
                        token_access : result.data.access,
                        token_refresh: result.data.refresh
                    }
                    this.$emit('completedSignUp', dataLogin)
                })
                .catch((error) => {
                    console.log(error);
                    alert("Error. Fallo en el registro de usuario.");
                }); 
            }
        }
    }
</script>

<style>
    .signUpUser{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .containerSignUpUser {
        border: 3px solid #283747;
        border-radius: 10px;
        width: 25%;
        height: 60%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .signUpUser h2{
        color: #283747;
    }

    .signUpUser form{
        width: 70%;
    }

    .signUpUser input{
        height: 40px;
        width: 100%;
        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;
        border: 1px solid #283747;
    }

    .signUpUser button{
        width: 100%;
        height: 40px;
        color: #E5E7E9;
        background: #283747;
        border: 1px solid #E5E7E9;
        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0 25px 0;
    }

    .signUpUser button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
    }
</style>