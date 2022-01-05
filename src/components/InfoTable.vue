<template>
  <div class="inner__info_table" :key="id" v-for="(v, id) in values">
    <div class="info_table_img">
      <img class="img" :src="v.img" alt="">
    </div>
    <div class="info_table_text">
      <div class="info_table_title">{{ v.name }}</div>
      <div class="info_table_lat">{{ v.lat }}</div>
      <div class="info_table_lat">{{ v.lng }}</div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
// import axios from 'axios'
export default {
  name: "InfoTable",
  props: ['infotable'],

  setup(props) {

    console.log(props.infotable)
    // const values = ref([])
    const values = ref(props.infotable)
    // if (props_values.value.length == 0) {
    //   axios.get(
    //     'http://127.0.0.1:8000/list/'
    //   ).then(response => values.value = response.data)
    //   console.log("0")
    // } else {
    //   console.log("1")
    //   for(let val of props_values.value) {
    //     console.log(val.name)
    //     values.value.push(
    //     {
    //       id : val.id,
    //       name : val.name,
    //       img : val.img,
    //       lat : val.lat,
    //       lng : val.lng
    //     }
    //     )
    //   }
    // }

    // const getGmapMarkerList = async () => {
    //    await axios.get(
    //      'http://127.0.0.1:8000/list/'
    //    ).then(response => values.value = response.data)
    //   // console.log(markers.value.data.results)
    // }
    // const val = ref(props.infotable)

    // onMounted(getGmapMarkerList)

    return {
      values
    }
  }
}
</script>

<style lang="css" scoped>
  .inner__info_table {
    width: 100%;
    height: 350px;
    padding-left: 10%;
  }

  .info_table_img {
    justify-content: center;
    width: 75%;
    height: 200px;
    margin: 10px 10px 10px 40px;
    border: 1px solid black;
  }

  .info_table_text {
    font-size: 16px;
    font-weight: 500;
    text-align: center;
  }

  .info_table_title {
    margin-top: 20px;
  }

  .info_table_lat {
    margin-top: 20px;
  }

  .img {
    height: 100%;
    width: 100%;
  }
</style>
