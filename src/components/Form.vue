<template>
    <div class="container">
        <form class="card" @submit.prevent="submit">
            <h1>Authorization</h1>

            <pre>{{ form.email }}</pre>
            <pre>{{ form.password }}</pre>

            <div class="form-control">
                <label for="email">Email</label>
                <input type="email" id="email" v-model="form.email.value">
            </div>
            
            
            <div class="form-control">
                <label for="password">Password</label>
                <input type="password" id="password" v-model="form.password.value">
            </div>

            <button class="button primary" type="submit">Submit</button>
        </form>


    </div>
</template>

<script>
import useForm from '../use/form';

const required = value => !!value
const minLength = num => val => val.length >= num

export default {
    name: 'Form',
    setup() {
        const form = useForm({
            email: {
                value: 'mail@mail.ru',
                validators: {
                    required
                }
            },
            password: {
                value: 'myPassword',
                validators: {
                    required, 
                    minLength: minLength(8)
                }

            }
        })

        console.log(form.password)

        function submit() {}

        return {form, submit}
    }
}
</script>