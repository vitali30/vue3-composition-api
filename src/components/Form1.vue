<template>
    <div class="container">
        <form class="card">

            <h1>Authorization</h1>

            <pre>{{ form }}</pre>

            <div class="form-control" :class="{ invalid : !form.email.isValidField && form.email.touched }">
                <label for="email">Email</label>
                <input type="email" id="email" v-model="form.email.value" @blur="form.email.blurInput">
                <small v-if="!form.email.fieldsRequirements.required">Eamil field is required</small>
                <small v-else-if="!form.email.fieldsRequirements.minLength">Eamil should be more or equival {{ emailMinLength }} symbols, now is: {{ form.email.value.length }}</small>
            </div>
            
            
            <div class="form-control"  :class="{ invalid : !form.password.isValidField && form.password.touched }">
                <label for="password">Password</label>
                <input type="password" id="password" v-model="form.password.value" @blur="form.password.blurInput">
                <small v-if="!form.password.fieldsRequirements.required">Password field is required</small>
                <small v-else-if="!form.password.fieldsRequirements.minLength">Password should be more or equival {{ passwordMinLength }} symbols, now is: {{ form.password.value.length }}</small>
            </div>

            <button class="button primary" type="submit">Submit</button>

        </form>
    </div>
</template>

<script>
import { reactive, ref, watch } from 'vue'

const required = val => !!val
const minLength = num => val => val.length >= num

export default {
    name: 'Form',
    setup() {
        const form = reactive({})

        const emailMinLength = 5;
        const passwordMinLength = 3


        const formObject = {
            email: {
                value: 'deff',
                validators: {
                    required,
                    minLength: minLength(emailMinLength)
                }
            },
            password: {
                value: 'defaultPassword',
                validators: {
                    required,
                    minLength: minLength(passwordMinLength)
                }
            }
        };

        for(const [key, value] of Object.entries(formObject)) {
            form[key] = useField(value)

        }

        form
        return {form, emailMinLength, passwordMinLength}

    }
}

function useField(field) {
    const value = ref(field.value);
    const isValidField = ref(true);
    const fieldsRequirements = reactive({});
    const touched = ref(false);

    const reassign = (value) => {
        for(const[key, validatorFunc] of Object.entries(field.validators)) {
            fieldsRequirements[key] = validatorFunc(value)

            isValidField.value = Object.values(fieldsRequirements)
                .filter(val => !val)
                .reduce((acc, val) => {
                    acc = val; 
                    return acc
                }, true)
        }
    }

    watch(value, reassign);

    reassign(field.value)

    const blurInput = () => touched.value = true

    return {value, isValidField, fieldsRequirements, touched, blurInput}

}
</script>