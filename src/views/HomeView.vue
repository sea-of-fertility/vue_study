<template>
  <div class="home">
    <InputBox @itemsFromInputBox="itemsFromChild" />
    <button @click="onDelete()">checked 삭제</button>
    <ul>
      <li v-for="(item, index) in items" :key="item.contentKey">
        {{ item.content }} <input type="checkbox" v-model="checkedValues[index]" />
      </li>
    </ul>

  </div>
</template>

<script>
// @ is an alias to /src
import InputBox from '@/components/InputBox.vue'
import axios from "axios";

export default {
  name: 'HomeView',
  components: {
    InputBox
  },
  data (){
    return{
      checkedValues : [],
      beDeletedItems : [],
      items : [],
    }
  },
  mounted() {
    axios.get('http://localhost:8081/getList')
        .then(res => {
          this.items = res.data;
          console.log(this.items);
        })
  },
  methods: {
    itemsFromChild (itemsFromInputBox) {
      this.items = itemsFromInputBox;
    },
    onDelete(){
      let deletedIndexList=[];
      for (let i = 0; i < this.checkedValues.length; i++) {
        if(this.checkedValues[i]===true){
          deletedIndexList.push(this.items[i].contentKey);
        }
      }

      axios.delete('http://localhost:8081/deleteItem', {data:{deletedIndexList}})
          .then(res => {
            console.log(res.data);
            this.checkedValues=[];
            this.resetItems();
          })
    },
    resetItems() {
      axios.get('http://localhost:8081/getList')
          .then(res => {
            this.items = res.data
          })
    }
  }

}
</script>
