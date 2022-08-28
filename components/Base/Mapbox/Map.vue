<template>
  <div class="ct-map">
    <div ref="map" class="ct-map__container" />
  </div>
</template>

<script>
import Mapbox from 'mapbox-gl'

export default {
  props: {
    configuration: {
      type: Object,
      default: () => ({})
    }
  },

  data () {
    return {
      loaded: false
    }
  },

  head () {
    return {
      link: [
        {
          rel: 'stylesheet',
          href: 'https://api.mapbox.com/mapbox-gl-js/v2.9.2/mapbox-gl.css'
        }
      ]
    }
  },

  mounted () {
    this.map = new Mapbox.Map({
      container: this.$refs.map,
      ...this.configuration
    })

    this.map.on('load', () => {
      this.map.addSource('my-data', {
        type: 'geojson',
        data: 'https://opendata.arcgis.com/api/v3/datasets/723ce658eb7b4f448e4d143b220c3273_0/downloads/data?format=geojson&spatialRefId=4326&where=1%3D1'
      })

      this.map.addLayer({
        id: 'species_markers',
        type: 'circle',
        source: 'my-data'
      })

      this.loaded = true
    })
  }
}
</script>

<style lang="postcss" scoped>
.ct-map {
  height: 512px;

  &__container {
    @apply h-full;
  }
}

</style>
