<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Регистрация</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Логин</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Email</label>
                        <div class="control">
                            <input type="text" class="input" v-model="email">
                        </div>
                    </div>

                    <div class="field">
                        <label>Пароль</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Повтор пароля</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password2">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Зарегистрироваться</button>
                        </div>
                    </div>

                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            email: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    methods: {
        submitForm() {
            this.errors = []

            if (this.username === '') {
                this.errors.push('Заполниет поле логина')
            }

            if (this.email === '') {
                this.errors.push('Заполниет поле email')
            }

            if (this.password === '') {
                this.errors.push('Заполниет поле пароля')
            }

            if (this.password !== this.password2) {
                this.errors.push('пароли не совпадают')
            }

            if (!this.errors.length) {
                const formData = {
                    username: this.username,
                    email: this.email,
                    password: this.password,
                }

                axios
                    .post("/auth/users/", formData)
                    .then(response => {
                        toast({
                            message: 'На указанную почту отправленно письмо для подтверждения учетной записи!',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 3000,
                            position: 'bottom-right',
                        })

                        this.$router.push('/log-in')
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }

                            console.log(JSON.stringify(error.response.data))
                        } else if (error.message) {
                            this.errors.push('Something went wrong. Please try again')
                            
                            console.log(JSON.stringify(error))
                        }
                    })
            }
        }
    }
}
</script>