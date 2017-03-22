<template>
  <f7-page>
    <f7-navbar>
      <f7-nav-left sliding>
        <f7-link @click="$router.back()">Back</f7-link>
      </f7-nav-left>
      <f7-nav-center>{{ $route.params.category }}</f7-nav-center>
    </f7-navbar>
    <f7-block inner>
      <ul>
        <li v-for="item in categoryItems"><f7-link :href='"/item/" + item.id + "/"'>{{ item.title }} {{ item.category }}</f7-link></li>
      </ul>
      <label for="title">Title</label>
      <input type="text" name="title" v-model="newItemTitle"><br>
      <button @click="createItem()">Add New Item</button>
    </f7-block>
  </f7-page>
</template>

<script>
  import store from 'assets/store.js'
  export default {
    data() {
      return {
        items: store.items,
        subItems: store.subItems,
        newItemTitle: ''
      }
    },
    computed: {
      categoryItems() {
        return this.items
          .filter(item => (item.category == this.$route.params.category))
      },
    },
    methods: {
      createItem() {
        function guid() {
          return s4() + s4();
        }

        function s4() {
          return Math.floor((1 + Math.random()) * 0x10000)
            .toString(16)
            .substring(1);
        }
        
        var newItemTitle = this.newItemTitle.trim(),
          newItemCategory = this.$route.params.category,
          uid = guid();
        
        store.items.push({
          title: newItemTitle,
          category: newItemCategory,
          id: uid
        })
        
        this.newItemTitle = '';
        
        this.$f7['mainView'].loadPage('/item/' + uid + '/');
      }
    }
  }
</script>