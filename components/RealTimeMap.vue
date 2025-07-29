<template>
    <div v-if="loading==false && emptyLocation==false">
        <div id="map-wrap" >
            <client-only>
            <l-map style="height: 600px" :zoom="zoom" :center="center">
                <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
                <l-marker :lat-lng="markerBus1" :icon="icon"></l-marker>
                <l-marker :lat-lng="markerBus2" :icon="icon"></l-marker>

                <l-polyline v-if="showTrail" :lat-lngs="trailBus1" color="#27ae60" weight="10"></l-polyline>
                <l-polyline v-if="showTrail" :lat-lngs="trailBus2" color="#27ae60" weight="10"></l-polyline>

            </l-map>
            </client-only>
        </div>
    </div>
    <div v-else-if="loading">
        <Loading />
    </div>
    <div v-else-if="emptyLocation">
      No hay información de la ubicación en tiempo real. Vuelva a intentarlo más tarde.
    </div>
</template>


<script>
import { latLng, icon } from "leaflet";

export default {


  props: {
      showTrail: {
          type: Boolean,
          default: false
      },
      centerAlways: {
          type: Boolean,
          default: false
      },
  },

  data () {
    return {
      trailBus1: [],
      trailBus2: [],
      firstTime: true,
      loading: true,
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 15,
      center: [51.505, -0.159],
      markerBus1: [51.504, -0.159],
      markerBus2: [51.506, -0.158],
      icon: icon({
        iconUrl: "images/bus_icon.png",
        iconSize: [50, 50],

      }),
      emptyLocation: true

    };
  },

  methods: {
    async fetchLocation(){

      const dataBus1 = await this.$axios.$get(`https://api.allorigins.win/get?url=https://www.interbusmurcia.es/bus1.php&timestamp=${new Date().getTime()}`)
      const dataBus2 = await this.$axios.$get(`https://api.allorigins.win/get?url=https://www.interbusmurcia.es/bus2.php&timestamp=${new Date().getTime()}`)

      const locationBus1 = dataBus1.contents.split(",").map(coord => parseFloat(coord.trim()));
      const locationBus2 = dataBus2.contents.split(",").map(coord => parseFloat(coord.trim()));

      console.log("Bus 1 location:", locationBus1);
      console.log("Bus 2 location:", locationBus2); 

      if(this.firstTime || this.centerAlways){
        // center is midpoint of both buses
        const lat1 = parseFloat(locationBus1[0]);
        const lng1 = parseFloat(locationBus1[1]);
        const lat2 = parseFloat(locationBus2[0]);
        const lng2 = parseFloat(locationBus2[1]);
        this.center = [
          (lat1 + lat2) / 2,
          (lng1 + lng2) / 2
        ];

        this.zoom = 10;
      }

      console.log


      if(locationBus1[0] != 0 && locationBus1[1] != 0){
        // Hay ubicacion
        this.markerBus1 = locationBus1;
        this.trailBus1.push(locationBus1);
        this.emptyLocation = false;
      }
      
      if(locationBus2[0] != 0 && locationBus2[1] != 0){
        // Hay ubicacion
        this.markerBus2 = locationBus2;
        this.trailBus2.push(locationBus2);
        this.emptyLocation = false;
      }
      

      this.loading=false;
      this.firstTime=false;

    },

    async locationInterval(){
      var ctx = this;
      ctx.fetchLocation();
      setInterval(function () {
        ctx.fetchLocation();
      }, 5000); // Cada 5 segundos
    }
  },
  mounted(){
    this.locationInterval();
  }
}
</script>