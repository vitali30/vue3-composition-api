<template>
<div>
  <h1>Hello in App</h1>
  <!-- <Form/> -->
  <pre>{{ this.$data }}</pre>
  <Child @someFunction="customFunction"/>
</div>
  
</template>

<script>
// import Form from './components/Form1.vue'
import Child from './child.vue'



export default {
  name: 'App',
  components: {
    // Form,
    Child
  },
  data() {
    return {
      a: 'string1',
      b: 2,
      c: {
        d: 'property1',
        e: 'property2',
        f: 1
      }
    }
  },
  methods: {
    customFunction() {
      this.c.f ++
      this.b ++
    }
  },
  created() {
    console.log(11111111111111111)
    this.$watch(() => this.c.f, (newValue, oldValue) => {console.log('property f in c object was changed. previous value was: ', oldValue, 'now: ', newValue)})
  },
}
</script>

