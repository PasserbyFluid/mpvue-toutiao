<template>
    <div class="counter-warp">
        <ul>
            <li v-for="(item,index) in list" :key="index">{{item}}</li>
        </ul>
    </div>
</template>

<script>
// Use Vuex
import store from './store'

export default {
    data() {
        return {
            list:[1,2,3,4]
        }
    },
    computed: {
        count () {
        return store.state.count
        }
    },
    methods: {
        increment () {
        store.commit('increment')
        },
        decrement () {
        store.commit('decrement')
        }
    }
}

</script>
<style>
.counter-warp {
  text-align: center;
  margin-top: 100px;
}
.home {
  display: inline-block;
  margin: 100px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}

</style>
