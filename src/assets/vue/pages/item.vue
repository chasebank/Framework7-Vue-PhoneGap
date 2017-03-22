<template>
  <f7-page>
    <f7-navbar>
      <f7-nav-left sliding>
        <f7-link @click="$router.back()">Back</f7-link>
      </f7-nav-left>
      <f7-nav-center>{{ item.title }}</f7-nav-center>
    </f7-navbar>
    <f7-block inner>
      <ul>
        <li v-for="subItem in itemSubItems">{{ subItem.title }}</li>
      </ul>
    </f7-block>
  </f7-page>
</template>

<script>
  import store from 'assets/store.js'
  export default {
    data() {
      return {
        items: store.items,
      }
    },
    computed: {
      item() {
        return this.items
          .find(item => (item.id == this.$route.params.itemId))
      },
    }
  }
</script>