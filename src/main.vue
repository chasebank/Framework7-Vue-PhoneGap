<template>
<!-- App -->
<div id="app">

  <!-- Statusbar -->
  <f7-statusbar></f7-statusbar>

  <f7-views>
    <f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
      <f7-navbar>
        <f7-nav-center>Categories</f7-nav-center>
      </f7-navbar>
      <f7-pages>
        <f7-page>
          <ul>
            <li v-for="category in categories"><f7-link :href='"/category/" + category + "/"'>{{ category }}</f7-link></li>
          </ul>

          <label for="title">Title</label>
          <input type="text" name="title" v-model="newItemTitle"><br>
          <label for="category">Category</label>
          <input type="text" name="category" v-model="newItemCategory"><br>
          <button @click="createItem()">Add New Item</button>
        </f7-page>
      </f7-pages>
    </f7-view>
  </f7-views>
  </div>
</template>

<script>
  import store from 'assets/store.js'
  
  export default {
    data() {
      return {
        items: store.items,
        newItemTitle: '',
        newItemCategory: ''
      }
    },
    computed: {
      categories() {
        const categoriesData = this.items.map(item => (item.category));
        
        var categoriesUnique = categoriesData
          .map((category) => {
            return {count: 1, category: category}
          })
          .reduce((a, b) => {
            a[b.category.toLowerCase()] = (a[b.category.toLowerCase()] || 0) + b.count
            return a
          }, {})
        
        // TODO sort by count
        return Object.keys(categoriesUnique)
      }
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
          newItemCategory = this.newItemCategory.trim(),
          uid = guid();
        
        store.items.push({
          title: newItemTitle,
          category: newItemCategory,
          id: uid
        })
        
        this.newItemTitle = '';
        this.newItemCategory = '';
        
        this.$f7['mainView'].loadPage('/item/' + uid + '/');
      }
    }
  }
</script>