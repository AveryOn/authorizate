<template>
    <div class="auth-block">
        <h1 class="auth-block__title">Authorization</h1>
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
            <button class="form--btn">Sign In</button>
            <p class="not-account">Don't have a account <a class="link">register</a></p>
        </form>
    </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

// Elements
const checkbox = ref<null | HTMLInputElement>(null);

// Boolean
const isRemember = ref<boolean>(false);

// Data
const checkboxValue = ref<string>('remember password');
const email = ref<string>('');
const password = ref<string>('');

// Functions
function triggerCheckbox():void {
    checkbox.value?.click();
}
function updateInput(e: any) { /* */ }

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
    display: flex;
    flex-direction: column;
    padding: 60px 30px 70px 30px;
    border-radius: 20px;
    border: 2px solid rgb(22, 160, 160);
    box-shadow: 6px 15px 25px rgba(0, 0, 0, 0.212);
}
.auth-block__title {
    position: relative;
    top: -10px;
    font-size: 42px;
    margin: 0 30px 30px 30px;
}

.auth-block__form {
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
}
.input:focus {
    background-color: #00000018;
    border-top-right-radius: 4px;
    border-top-left-radius: 4px;
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.15); */
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
    margin-top: 10px;
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
}
.not-account {
    font-size: 16px;
    align-self: center;
    margin-top: 20px;
    /* color: rgb(40, 146, 146); */
    font-weight: bold;
}
.link {
    color: rgb(151, 4, 151);
    text-decoration-line: underline;
    text-decoration-color: rgb(151, 4, 151);
    cursor: pointer;
}
.link:hover {
    color: rgb(198, 56, 198);
    text-decoration-color: rgb(198, 56, 198);
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
