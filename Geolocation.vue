<script>
export default {
  name: 'Geolocation',
  props: {
    // will start getCurrentPosition method on component mounted
    // if false getCurrentPosition method can be consumed by the "slot-scope"
    auto: {
      type: Boolean,
      default: true
    },
    // will launch watchPosition or getCurrentPosition method
    watch: {
      type: Boolean,
      default: false
    },
    // default values from https://developer.mozilla.org/fr/docs/Web/API/PositionOptions
    positionOptions: {
      type: Object,
      default () {
        return {
          enableHighAccuracy: false,
          timeout: Infinity,
          maximumAge: Infinity
        }
      }
    }
  },
  data () {
    return {
      currentLocation: {
        coords: {
          latitude: null, // The latitude as a decimal number (always returned)
          longitude: null, // The longitude as a decimal number (always returned)
          accuracy: null, // The accuracy of position (always returned)
          altitude: null, // The altitude in meters above the mean sea level (returned if available)
          altitudeAccuracy: null, // The heading as degrees clockwise from North (returned if available)
          heading: null, // The heading as degrees clockwise from North (returned if available)
          speed: null // The speed in meters per second (returned if available)
        },
        timestamp: null // The date/time of the response (returned if available)
      },
      error: null,
      loading: false
    }
  },
  methods: {
    getCurrentPosition () {
      if (navigator.geolocation) {
        // test for HTTPS
        if (window.location.protocol === 'http:') console.warn('Geolocation API may not work without HTTPS protocol')
        this.loading = true
        if (this.watch) {
          this._id = navigator.geolocation.watchPosition(this.showPosition, this.showError, this.positionOptions)
        } else {
          navigator.geolocation.getCurrentPosition(this.showPosition, this.showError, this.positionOptions)
        }
      } else {
        this.error = new Error('No geolocation API')
      }
    },
    showPosition ({ coords }) {
      this.currentLocation.coords.latitude = coords.latitude
      this.currentLocation.coords.longitude = coords.longitude
      this.loading = false
    },
    showError (error) {
      this.error = error
      this.loading = false
    }
  },
  watch: {
    currentLocation (newVal) {
      this.$emit('input', newVal)
    },
    error (newVal) {
      this.$emit('geolocation_error', newVal)
    },
    loading (newVal) {
      this.$emit('geolocation_loading', newVal)
    }
  },
  render () {
    return this.$scopedSlots.default({
      currentLocation: this.currentLocation,
      error: this.error,
      loading: this.loading,
      getCurrentPosition: this.getCurrentPosition
    })
  },
  mounted () {
    if (this.auto) {
      this.getCurrentPosition()
    }
  },
  destroyed () {
    if (this._id) {
      navigator.geolocation.clearWatch(this._id)
    }
  }
}
</script>
