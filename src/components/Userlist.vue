<template>
  <div>
    <h1>This is User List</h1>

    <input type="text" placeholder="Search Using Title" v-model="search_item" />

    <div v-for="item in filter" :key="item.id" style="margin-bottom: 20px">
      {{ item.id }} => Title: {{ item.title }} <br />
      body: {{ item.body }}
      <button
        type="button"
        class="btn"
        v-on:click="showModal(item.id, item.title, item.body)"
      >
        Edit
      </button>
      <Delete :array_key="item.id" @delete_post="delete_post($event)" />
      <Edit
        v-show="isModalVisible"
        v-on:close="closeModal"
        :title="title"
        :array_key="index"
        :body="body"
        @update_post="update_post($event)"
      />
    </div>
  </div>
</template>
<script>
import Delete from "./Delete.vue";
import Edit from "./edit.vue";
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);
export default {
  components: { Edit, Delete },
  name: "Userlist",
  data() {
    return {
      list: null,
      search_item: null,
      title: null,
      index: null,
      body: null,
      isModalVisible: false,
    };
  },
  methods: {
    showModal(index, title, body) {
      this.title = title;
      this.index = index;
      this.body = body;
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    update_post(value) {
      Vue.axios.put(
        "https://jsonplaceholder.typicode.com/posts/" + value.array_index,
        {
          title: value.title,
          body: value.body,
        }
      );
      for (var i = 0; i < this.list.length; i++) {
        if (this.list[i].id == value.array_index) {
          this.list[i].title = value.title;
          this.list[i].body = value.body;
        }
      }
      this.$swal({
        position: "top-end",
        icon: "success",
        title: "Post Updated Successfully",
        showConfirmButton: false,
        timer: 1500,
      });
    },
    delete_post(delete_key) {
      Vue.axios.delete(
        "https://jsonplaceholder.typicode.com/posts/" + delete_key
      );
      for (var i = 0; i < this.list.length; i++) {
        if (this.list[i].id == delete_key) {
          this.list.splice(i, 1);
        }
      }
      this.$swal({
        position: "top-end",
        icon: "success",
        title: "Post Deleted Successfully",
        showConfirmButton: false,
        timer: 1500,
      });
    },
  },
  mounted() {
    Vue.axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((result) => {
        this.list = result.data;
      });
  },

  computed: {
    filter: function () {
      var items = this.list;
      var search = this.search_item;
      if (!this.search_item) {
        return items;
      }
      search = search.trim().toLowerCase();
      items = items.filter(function (item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });

      return items;
    },
  },
};
</script>