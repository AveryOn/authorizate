<template>
    <div class="auth-block">
        <div class="notification">
            {{ message }}
        </div>
        <!-- Authorizate -->
        <div class="wrapper" id="wrapper-authorizate" v-show="isAuthorizate">
            <h1 class="auth-block__title">Authorization</h1>
            <form class="auth-block__form" action="" @submit.prevent>
                <!-- Field (login) -->
                <div class="form--field">
                    <input 
                    class="input" 
                    type="email" 
                    placeholder="login"
                    v-model="login"
                    @input="validateEmail(login)"
                    >
                    <span class="material-symbols-outlined">
                        alternate_email
                    </span>
                </div>
    
                <!-- Field (password) -->
                <div class="form--field">
                    <input 
                    class="input" 
                    type="password" 
                    placeholder="password"
                    v-model="password"
                    >
                    <span class="material-symbols-outlined">
                        lock
                    </span>
                </div>
    
                <!-- Field (forget pswrd) -->
                <div class="forget-password">
                    <input 
                    type="checkbox" 
                    ref="checkbox" 
                    id="checkbox" 
                    :value="checkboxValue"
                    v-model="isRemember"
                    @change="e => updateInput(e)"
                    >
                    <p class="forget-msg" @click="triggerCheckbox">Remember Me forget password</p>
                </div>
                <button class="form--btn" @click="signIn">Sign In</button>
                <p class="not-account">Don't have a account 
                    <a class="link" @click="activeRegistration">
                        register
                    </a>
                </p>
            </form>
        </div>

        <!-- Register -->
        <div class="wrapper" id="wrapper-register" v-show="isRegister">
            <h1 class="auth-block__title">Sign Up</h1>
            <form class="auth-block__form" action="" @submit.prevent>
                <!-- Field (email) -->
                <div class="form--field">
                    <input 
                    class="input" 
                    type="email" 
                    placeholder="email"
                    v-model="email"
                    >
                    <span class="material-symbols-outlined">
                        alternate_email
                    </span>
                </div>
    
                <!-- Field (password) -->
                <div class="form--field">
                    <input 
                    class="input" 
                    type="password" 
                    placeholder="password"
                    v-model="password"
                    >
                    <span class="material-symbols-outlined">
                        lock
                    </span>
                </div>
    
                <button class="form--btn" id="btn-sign-up" @click="signUp">Sign Up</button>
                <p class="already-account">Already have an account? 
                    <a class="link" @click="activeAuthorizate">
                        sign in ->
                    </a>
                </p>
            </form>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import gsap from 'gsap';
import { register } from 'register-service-worker';

// Elements
const checkbox = ref<null | HTMLInputElement>(null);

// Boolean
const isRemember = ref<boolean>(false);
const isRegister = ref<boolean>(false);
const isAuthorizate = ref<boolean>(true);

// Data
const checkboxValue = ref<string>('remember password');
const email = ref<string>('');
const login = ref<string>('');
const password = ref<string>('');
const message = ref<string>('Это нахуй текст');

// Functions
function triggerCheckbox():void {
    checkbox.value?.click();
}
function updateInput(e: any) { /* */ }

function validateEmail(email: string): boolean {
    const regex = /^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]{2,7}$/;
    // console.log(regex.test(email));
    return regex.test(email);
}

const tl = gsap.timeline();
function pushNotification(msg: string) {
        message.value = msg;
        tl.to('.notification', { left: '30px', visibility: 'visible', duration: 0.8 })
        tl.to('.notification', { left: '600px', duration: 0.8, delay: 2.5 })
        tl.to('.notification', { left: '-600px', visibility: 'hidden', duration: 0.001 })
        tl.then(() => {
            message.value = '';
        })
}

// Authantificate
async function signIn() {
    // Checking Fields
    if(!login.value || !password.value) {
        pushNotification('Fill in all the fields');
        return;
    }
    // Checking password; 
    if(password.value.length < 8) {
        pushNotification('Password less than 8 characters!')
        return;
    }
    /* data request to server successfully */
    console.log('Sending data to the server...');
}

// Registration
async function signUp() {
        // Checking Fields
        if(!email.value || !password.value) {
        pushNotification('Fill in all the fields');
        return;
    }
    // Checking Email;
    if(!validateEmail(email.value)){
        pushNotification('Invalid email')
        return;
    } 
    // Checking password; 
    if(password.value.length < 8) {
        pushNotification('Password less than 8 characters!')
        return;
    }
    /* data request to server successfully */
    console.log('Sending data to the server...');
}

// Включить окно РЕГИСТРАЦИИ
function activeRegistration(){
    Promise.resolve(gsap.to('#wrapper-authorizate', { bottom: '80vh', duration: 0.4 }))
        .then(() => isAuthorizate.value = false)
        .then(() => isRegister.value = true)
        .then(() => gsap.to('#wrapper-register', { bottom: '0vh', duration: 0.4 }))
}
function activeAuthorizate() {
    Promise.resolve(gsap.to('#wrapper-register', { bottom: '-80vh', duration: 0.4 }))
        .then(() => isRegister.value = false)
        .then(() => isAuthorizate.value = true)
        .then(() => gsap.to('#wrapper-authorizate', { bottom: '0vh', duration: 0.4 }))
}
</script>

<style>
:root {
    box-sizing: border-box;
}
* {
    padding: 0;
    margin: 0;
    box-sizing: inherit;
}
body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: linear-gradient(90deg, #6dffd6, #708fff);
    height: 100vh;
    color: white;
    text-shadow: 1px 1px 4px #708fff;
    font-family: monospace;
}
.auth-block {
    position: relative;
    width: 420px;
    height: 420px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    padding: 60px 30px 70px 30px;
    border-radius: 20px;
    border: 2px solid rgb(22, 160, 160);
    box-shadow: 6px 15px 25px rgba(0, 0, 0, 0.212);
}
.notification {
    display: flex;
    position: absolute;
    visibility: hidden;
    left: -600px;
    bottom: 20px;
    align-items: center;
    justify-content: center;
    width: max-content;
    height: max-content;
    padding: 5px 15px;
    border-radius: 5px;
    background: linear-gradient(90deg, #f75224c1, #ad1a0aba);
    color: rgb(254, 203, 108);
    font-weight: normal;
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    letter-spacing: 0.7px;
}
.wrapper {
    position: relative;
}
#wrapper-register {
    bottom: -80vh;    
}
.auth-block__title {
    position: relative;
    top: -10px;
    font-size: 42px;
    margin: 0 30px 30px 30px;
}
.auth-block__form {
    position: relative;
    display: flex;
    flex-direction: column;
}
.input {
    width: 100%;
    background: transparent;
    outline: rgba(0,0,0,0);
    padding: 8px 18px;
    border: none;
    border-bottom: 2px solid rgb(22, 160, 160);
    color: rgb(255, 255, 255);
    font-weight: bold;
    font-size: 16px;
    transition: 1s;
}
.input:focus {
    background-color: #00000018;
    border-top-right-radius: 4px;
    border-top-left-radius: 4px;
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.15); */
}

.input:hover {
    background-color: #00000018;
    transition: 1s;
}
.input::placeholder {
    color: rgba(225, 221, 227, 0);
    font-weight: bolder;
    text-shadow: 1px 1px 2px rgb(17, 133, 133);
    font-family: monospace;
    font-size: 18px;
}
.form--field {
    position: relative;
    display: flex;
    align-items: center;
}
.form--field + .form--field {
    margin-top: 15px;
}
.forget-password {
    display: flex;
    flex-wrap: wrap;
    font-size: 16px;
    font-family: monospace;
    margin: 15px 0 0 10px;
    letter-spacing: .5px;
    /* color: rgb(40, 146, 146); */
}
.forget-msg {
    margin-left: 8px;
    cursor: pointer;
}
.form--btn {
    align-self: flex-end;
    margin-top: 10px;
    background: transparent;
    outline: rgba(0,0,0,0);
    padding: 8px 18px;
    border: 1px solid rgb(22, 160, 160);
    border-radius: 8px;
    color: white;
    font-weight: bolder;
    font-size: 15px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    letter-spacing: 1px;
    cursor: pointer;
    transition: 1s;
}
.form--btn:hover {
    background-color: #00000018;
    transition: 1s;
}
#btn-sign-up {
    margin-top: 20px;
}
.not-account {
    font-size: 16px;
    align-self: center;
    margin-top: 20px;
    font-weight: bold;
}
.already-account {
    font-size: 16px;
    align-self: center;
    margin-top: 30px;
    font-weight: bold;
}
.link {
    color: rgb(151, 4, 151);
    text-decoration-line: underline;
    text-decoration-color: rgb(151, 4, 151);
    cursor: pointer;
    transition: 1s;
}
.link:hover {
    color: rgb(198, 56, 198);
    text-decoration-color: rgb(198, 56, 198);
    transition: 1s;
}
.material-symbols-outlined {
    position: absolute;
    right: 5px;
    font-variation-settings:
    'FILL' 0,
    'wght' 300,
    'GRAD' 0,
    'opsz' 24
}
</style>
