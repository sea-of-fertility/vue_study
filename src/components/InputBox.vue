<template>
  <div class="hello">
    <textarea v-model="item"/> <button @click="onSave()">저장</button>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: 'InputBox',
    data() {
      return{
        item: "",
    }
  },
    methods: {
      onSave() {
        axios.put('http://localhost:8081/saveList', {newContent: this.item})
            .then(res => {
              console.log(res.data)
              this.resetItems();
            })
      },
      resetItems() {
        axios.get('http://localhost:8081/getList')
            .then(res => {
              this.$emit("itemsFromInputBox", res.data) ;
              console.log(this.items);
            })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
