<template>
  <div class="home">
    <InputBox/>
  </div>

</template>

<script>
// @ is an alias to /src
import InputBox from '@/components/InputBox.vue'
import axios from "axios";

export default {
  name: 'HomeView',
  data (){
    return{
      items : [],
    }
  },
  components: {
    InputBox
  },
  methods: {
    computed() {
      axios.get('http://localhost:8081/getList')
          .then(res => {
            console.log(res.data)
          })
    }
  }

}
</script>
