<template>
    <div class="container">
        <form class="card" @submit.prevent="submit">
            <h1>Authorization</h1>

            <pre>{{ form }}</pre>

            <div class="form-control" :class="{ invalid: !form.email.valid && form.email.touched }">
                <label for="email">Email</label>
                <input class="invalid" type="email" id="email" v-model="form.email.value" @blur="form.email.blur">
                <small v-if="form.email.touched && form.email.errors.required">Email field is required</small>
            </div>
            
            
            <div class="form-control" :class="{ invalid: !form.password.valid && form.password.touched }">
                <label for="password">Password</label>
                <input type="password" id="password" v-model="form.password.value" @blur="form.password.blur">
                <small v-if="form.password.touched && form.password.errors.required">Password field is required</small>
                <small v-else-if="form.password.touched && form.password.errors.minLength">password length can`t be less than 8, now it is {{ form.password.value.length }}</small>
            </div>

            <button class="button primary" type="submit" :disabled="!form.valid">Submit</button>
        </form>


    </div>
</template>

<script>
import { reactive, computed, ref, watch} from 'vue';

const required = function(value) {return !!value}
const minLength = num => val => val.length >= num

export default {
    name: 'Form',
    setup() {
        const formObject = {//рыба обьекта формы
            email: {
                value: '',
                validators: {
                    required
                }
            },
            password: {
                value: '',
                validators: {
                    required, 
                    minLength: minLength(8)
                }

            }
        };

        const form = useForm(formObject)//сам обьект формы

        function submit() {
            console.log('email: ', form.email.value)
            console.log('password: ', form.password.value)
        }

        return {form, submit}
    }
}

function useForm(formObject = {}) {
    
    const form = reactive({});
    const validKey = 'valid'

    for (const [key, value] of Object.entries(formObject)) {
        form[key] = useField(value)
    }

    const withoutValid = k => k != validKey


    form.valid = computed(() => {
        return Object.keys(form).filter(withoutValid).reduce((acc, key) => {
            acc = form[key].valid;
            return acc
        }, true)
    })

    return form
}



const not = val => !val

function useField(field) {
    const valid = ref(true);
    const value = ref(field.value);
    const touched = ref(false);
    const errors = reactive({});

    const reassign = val => {
        valid.value = true

        Object.keys(field.validators ?? {}).map(name => {
            console.log(name);
            const isValid = field.validators[name](val);
            errors[name] = not(isValid)
    
            if(not(isValid)) {
                valid.value = false
            }
        })
    }

    

    watch(value, reassign)

    reassign(field.value)

    return {valid, value, errors, touched, blur: () => touched.value = true}
}
</script>