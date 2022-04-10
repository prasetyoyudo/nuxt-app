<template>
  <UserCreate/>
</template>

<script lang="ts">
import Vue from 'vue'
import UserCreate  from '../components/UserCreate.vue'

export default Vue.extend({
  name: 'IndexPage',

  components: {
    UserCreate
  }
})
</script>
