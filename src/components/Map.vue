<template lang="html">
  <div class="wrapper">
    <div class="map">
        <GMapMap class ="vue-map"
            :center=center
            :zoom="6"
            map-type-id="roadmap"
            ref="myMapRef"
        >
        <GMapCluster class="map-cluster"
          :minimumClusterSize="2"
          :zoomOnClick="true"
          :styles="clusterIcon"
          :averageCenter_="true"
          @click="toggleClusterInfo"
          >
          <GMapMarker
            :key="index"
            v-for="(m, index) in markers"

            :position="m"
            :icon='{
                url: "https://www.gup.ua/images/marker-map-blue.svg",
                scaledSize: {width: 30, height: 30},
                labelOrigin: {x: 16, y: -10}
            }'
             @click="getLockationIngo(m)"
            :clickable="true"


          />
        </GMapCluster>
        </GMapMap>
    </div>
    <div class="info-table">
      <InfoTable :infotable="values" />
    </div>

    <div class="addForm">
      <p>Add new good</p>
      <label for="name">Title</label>
      <input class="input_text" type="text" id="name" required v-model="name">
      <label for="pass">Image path</label>
      <input class="input_text" type="file" id="image" name="img" required v-on:change="fileSend">
      <label for="latitude">latitude</label>
      <input class="input_text" type="text" id="lat" required v-model="lat">
      <label for="longitude">longitude</label>
      <input class="input_text" type="text" id="lng" required v-model="lng">
      <div class="input_btn">
        <button type="button" name="button" @click="sendGood()">Send</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import InfoTable from "../components/InfoTable"
import axios from 'axios'

export default {
  name: "Map",
  components: {
    InfoTable,
  },

  setup() {

    const name = ref('')
    const img = ref('')
    const lat = ref('')
    const lng = ref('')

    const values = ref([])

    const clusterIcon = [
      {
        textColor: 'white',
        url: 'https://github.com/googlemaps/v3-utility-library/blob/master/markerclusterer/images/m1.png?raw=true',
        height: 50,
        width: 50,
        textAlign: center,
      },
    ]
    const center = {lat: 50.4546600, lng: 30.5238000}
    const markers = ref([])

    const getGmapMarkerList = async () => {
       await axios.get(
         'https://gmap-django.herokuapp.com/list/'
       ).then(response => markers.value = response.data)

      // console.log(markers.value)
      // console.log(values.value)

      for(let val of markers.value) {
        console.log(val.name)
        values.value.push(
        {
          id : val.id,
          name : val.name,
          img : val.img,
          lat : val.lat,
          lng : val.lng
        }
        )
      }

    }
    onMounted(getGmapMarkerList)
    const toggleClusterInfo = (cluster) => {
      let markers_positions = []
      console.log(markers)
      for (let i in cluster.markers_) {
        let pos_arr = []
        pos_arr.push(cluster.markers_[i].position.lat())
        pos_arr.push(cluster.markers_[i].position.lng())
      markers_positions.push(pos_arr)
      console.log(markers_positions)
      }

      exists(markers_positions)
      return markers_positions
    }

    const exists = (markers_positions) => {
      console.log(markers)
      console.log(markers_positions)
      values.value.splice(0, values.value.length)

      for (let i in markers_positions){
        for (let j in markers.value) {
          if (markers.value[j].lat == markers_positions[i][0] || markers.value[j].lng == markers_positions[i][1]) {
            values.value.push(
              {
                id : markers.value[j].id,
                name : markers.value[j].name,
                img : markers.value[j].img,
                lat : markers.value[j].lat,
                lng : markers.value[j].lng,
              }
            )
            console.log(values.value)
          }
        }
      }
    }

    const getLockationIngo = (m) => {

      values.value.splice(0, values.value.length)

      values.value.push(
        {
          id : m.id,
          name : m.name,
          img : m.img,
          lat : m.lat,
          lng : m.lng,
        }
      )
      console.log(m)
      return values

    }

    const fileSend = (event) => {
      img.value = event.target.files[0]
      console.log(img.value)
    }

    const sendGood = async () => {
      const formData = new FormData();
      formData.append("name", name.value)
      formData.append("lat", lat.value)
      formData.append("lng", lng.value)
      formData.append("img", img.value)

      const config = {
        headers: {
          'Content-Type': 'multipart/form-data; boundary=something'
        }
      }
      axios.post('https://gmap-django.herokuapp.com/create/', formData, config)

      // name.value = ""
      // lat.value= ""
      // lng.value=""
    }

    return {
      getGmapMarkerList,
      center,
      markers,
      getLockationIngo,
      values,
      clusterIcon,
      toggleClusterInfo,
      // exists,
      sendGood,
      name,
      img,
      lat,
      lng,
      fileSend
    }
  }
}
</script>

<style scoped>
  .map {
    width: 75%;
    height: 750px;
  }

  .info-table {
    width: 25%;
  }

  .vue-map {
    width: 100%;
    height: 100%;
  }

  .test {
    width: 75%;
    height: 750px;
  }

  .map-cluster {

  }

  .wrapper {
    display: flex;
    position: relative;
  }

  .addForm {
    padding-top: 320px;
    max-width: 350px;
    margin: 0 auto;
    position: absolute;
    top: 480px;
    left: 45%;
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
    text-transform: uppercase;
    font-size: 12px;
  }

  p {
    margin-left: 40px;
  }

</style>

<!-- {
  id: 1,
  img: 'https://playo.imgix.net/HARSHACRICKETGROUND/2.PNG?auto=compress,format&h=300',
  name: "Ground",
  lat: 46.12,
  lng: 31.321
},
{
  id: 2,
  img: 'https://playo.imgix.net/HARSHACRICKETGROUND/2.PNG?auto=compress,format&h=300',
  name: "Ground",
  lat: 46.122444,
  lng: 31.32123
},
{
  id: 3,
  img: 'https://playo.imgix.net/HARSHACRICKETGROUND/2.PNG?auto=compress,format&h=300',
  name: "Ground",
  lat: 46.12333,
  lng: 31.321241
},
{
  id: 4,
  img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSddflulvnY1sM74iww7XOzZqZmb6mF7xkYmA&usqp=CAU',
  name: "Car",
  lat: 46.97503,
  lng: 31.99458
},
{
  id: 5,
  img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSddflulvnY1sM74iww7XOzZqZmb6mF7xkYmA&usqp=CAU',
  name: "Car",
  lat: 46.97913,
  lng: 31.99758
},
{
  id: 6,
  img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSddflulvnY1sM74iww7XOzZqZmb6mF7xkYmA&usqp=CAU',
  name: "Car",
  lat: 46.96103,
  lng: 31.992358
},
{
  id: 7,
  img: "https://cdn.vox-cdn.com/thumbor/KrNjaW263LdvsGPRg1valjIn_rI=/1400x1050/filters:format(jpeg)/cdn.vox-cdn.com/uploads/chorus_asset/file/21818147/iStock_172413371.jpg",
  name: "Build",
  lat: 47.213,
  lng: 31.238
}, -->
