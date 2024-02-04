<template>
  <div id="app">
    <div class="container">
      <MySelect class="test" :options="options" v-model="checked"></MySelect>
    </div>
  </div>
</template>

<script>
import MySelect from '@/components/MySelect.vue'
export default {
  name: 'App',
  components: {MySelect},
  data: () => ({
    options: [
      {
        key: 1,
        name: 'Option1'
      },
      {
        key: 2,
        name: 'Option2'
      },
      {
        key: 3,
        name: 'Option3'
      },
      {
        key: 4,
        name: 'Option4'
      },
      {
        key: 5,
        name: 'Option5'
      },
      {
        key: 6,
        name: 'test'
      },
      {
        key: 7,
        name: 'text'
      },
    ],
    checked: [{
        key: 3,
        name: 'Option3'
      },]
  })
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container{
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
