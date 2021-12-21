<template>
  <div class="container">
      <Map />
  </div>
</template>

<script>
import Map from "../components/Map"
// import InfoTable from "../components/InfoTable"
import { ref } from 'vue'

export default {

  components: {
    Map,
    // InfoTable,
  },

  setup() {
    const data = ref({})
    const name = ref('')
    const img = ref('')
    const lat = ref('')
    const lng = ref('')

    const sendGood = () => {
      data.value.img = img.value,
      data.value.name = name.value
      data.value.lat =  lat.value,
      data.value.lng = lng.value


      // let data = {
      //   id: 123,
      //   img: img.value,
      //   name: name.value,
      //   lat: lat.value,
      //   lng: lng.value
      // }
      console.log(data)
      return data
    }

    return {
      sendGood,
    }

  }
}
</script>

<style lang="css" scoped>
  .container {
    width: 100%;
    height: 2000px;
  }

  /* .map-wrapper {
    width: 100%;
    height: 500px;
    display: flex;
  }

  .addForm {
    padding-top: 320px;
    max-width: 350px;
    margin: 0 auto;
  }

  .input_text {
    margin-bottom: 10px;
  }

  .input_btn {
    margin-top: 10px;
    margin-left: 70px;
  }

  label {
    display: block;
    width: 100%;
    /* margin-bottom: 5px; */
    /* color: #FAAB1B; */
    /* text-transform: uppercase;
    font-size: 12px;
  }

  p {
    margin-left: 40px;
  } */

  /* .map {
    width: 75%;
    height: 750px;
  }

  .info-table {
    width: 25%;
  } */
</style>
