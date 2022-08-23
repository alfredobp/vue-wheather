<template>
  <div class="" v-if="!error">
    <map-block
      :location="[data.location.lat, data.location.long]"
      v-if="data"
    ></map-block>

    <div class="col-md-6" style="width: 50%; margin: 0 auto">
      <div v-if="data"  :class="data.current_observation.condition.text ">
    <h1 id="title">Información meteorológica del municipio</h1>
        <h1 is="sui-segment" inverted>
          Previsión Meterológica para: {{ data.location.city }}
        </h1>
        <sui-divider horizontal>Datos viento</sui-divider>
        <div class="ui vertically divided grid">
          <div class="three column row">
            <div class="column">
              <p>
                Sensación térmica:
                {{ (((data.current_observation.wind.chill - 32) * 5) / 9).toFixed(2) }}
                ªC
              </p>
            </div>
            <div class="column">
              <p>
                <img src="@/assets/direccion-del-viento.png" width="30px" />
                {{ data.current_observation.wind.direction }}
              </p>
            </div>
            <div class="column">
              <p>
                <img src="@/assets/presion.png" width="30px" />

                {{ data.current_observation.wind.speed }}
              </p>
            </div>
          </div>
        </div>
        <sui-divider horizontal>Datos Atmósfericos</sui-divider>
        <div class="ui vertically divided grid">
          <div class="three column row">
            <div class="column">
              <p>
                <img src="@/assets/presion.png" width="30px" />
                {{ data.current_observation.atmosphere.pressure }}
              </p>
            </div>
            <div class="column">
              <p>
                <img src="@/assets/visibilidad.png" width="30px" />
                {{ data.current_observation.atmosphere.visibility }}
              </p>
            </div>
            <div class="column">
              <p>
                <img src="@/assets/humedad.png" width="30px" />
                {{ data.current_observation.atmosphere.humidity }}
              </p>
            </div>
          </div>
        </div>
        <sui-divider horizontal>Datos Actuales</sui-divider>

        <div class="ui vertically divided grid">
          <div class="two column row">
            <div class="column">
              <p>
                <img src="@/assets/thermometer.png" width="30px" />
                Tª:
                {{
                  (
                    ((data.current_observation.condition.temperature - 32) * 5) /
                    9
                  ).toFixed(2)
                }}
                ªC
              </p>
            </div>
            <div class="column">
              <p>
                <span v-if="data.current_observation.condition.code == 34">
                  Soleado

                  <img src="@/assets/dom.png" width="30px" />
                </span>
              </p>
            </div>
          </div>
        </div>
      <wiki-block :nameCity="'Sanlucar de barrameda'"></wiki-block>
      </div>
      <div v-else-if="error">

          <p>No se han podido recabar datos.</p>
      </div>
    </div>
  </div>
  <div v-else>Se ha producido un error</div>
</template>

<script>
import MapBlock from "@/components/MapBlock";
import wikiBlock from "@/components/WikiData";

export default {
  name: "MeteoData",
  components: {
    mapBlock: MapBlock,
        wikiBlock: wikiBlock,
  },
  props:
     ["data", "error","background"],
};

</script>

<style v-if="data.current_observation.condition.text == 'sunny'">
/* body{
      background-image: url('../assets/summer.jpg');
} */
</style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
