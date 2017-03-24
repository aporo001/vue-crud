<!-- /components/Home.vue -->
<template>
  <div>
    <h1>Home</h1>
    <button type="button" class="btn btn-default btn-lg" @click="goAdd">
      <span class="glyphicon glyphicon-plus" aria-hidden="true" ></span> Add
    </button>
    <br />
    <br />
    <div class="col-sm-6 col-md-4" v-for="item in list">
      <div class="thumbnail">
        <img :src="item.url" :alt="item.title">
        <div class="caption">
          <h3>{{ item.title }}</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const API_URL = 'http://localhost:5000/photos'

export default {
  name: 'Home',
  data: function () {
    return {
      list: []
    }
  },
  methods: {
    goAdd: function () {
      this.$router.push('/add')
    },
    loadDate: function () {
      this.$http.get(API_URL)
        .then(response => {
          console.log(response)
          this.list = response.body
        }, () => {
          this.list = []
        })
    }
  },
  created: function () {
    this.loadDate()
  }
}
</script>

