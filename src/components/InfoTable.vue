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
export default {
  name: "InfoTable",
  props: ['infotable'],

  setup(props) {
    // const values = ref([])
    console.log(props.infotable)
    const values = ref(props.infotable)
    // const a = ref(props.infotable)
    // console.log(a)


    return {
      values,
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
