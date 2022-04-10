<template>
  <UserDetail/>

</template>

<script lang="ts">
import Vue from 'vue'
import UserDetail  from '../components/UserDetail.vue'

export default Vue.extend({
  name: 'IndexPage',

  components: {
    UserDetail
  }
})
</script>
