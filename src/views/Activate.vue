<template>
    <div class="page-activate">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Учетная запись активирована!</h1>
                <router-link to="/log-in">click here</router-link> to log in!
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Activate',
    created() {
        this.ActivateUser()
    },
    mounted() {
        document.title = 'Activate'
    },
    methods: {
        ActivateUser() {

            const actData = {
                uid: this.$route.params.uid,
                token: this.$route.params.token
            }
                axios
                    .post("/auth/users/activation/", actData)

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
</script>