<template>
    <div class="container">
        <form class="card">

            <h1>Authorization</h1>

            <div class="form-control">
                <label for="email">Email</label>
                <input class="invalid" type="email" id="email">
            </div>
            
            
            <div class="form-control">
                <label for="password">Password</label>
                <input type="password" id="password">
            </div>

            <button class="button primary" type="submit">Submit</button>

        </form>
    </div>
</template>

<script>

export default {
    name: 'Form',
    setup() {
        const formObject = {
            email: {
                value: 'defaultEmail'
            },
            password: {
                value: 'defaultPassword'
            }
        };

        const form = formChanging(formObject)
    }
}

function formChanging(formObject = {}) {
    
}


</script>