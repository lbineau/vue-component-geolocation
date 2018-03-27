# vue-component-geolocation
Vue renderless component to use Geolocation API using [scoped slots](https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots)

![notification](https://image.ibb.co/fXERn7/screenshot.png)
![button](https://image.ibb.co/ecGHun/button.png)

Installation
------
```javascript
import Geolocation from 'vue-component-geolocation'
```

Usage
------
This component is renderless so it doesn't impose any decisions about design or layout. You are free to use it as you wish.

### Simple usage
```vue
<geolocation></geolocation>
```
### More complex usage
```vue
<geolocation :auto="false">
  <div slot-scope="{ getCurrentPosition, currentLocation, loading, error }"><!-- slot-scope allow to use methods and properties -->
    <div>
      <div v-if="error">{{error.code}}: {{error.message}}</div>
      <div v-if="currentLocation.coords.latitude && currentLocation.coords.longitude">{{currentLocation.coords.latitude}}: {{currentLocation.coords.longitude}}</div>
      <button @click="getCurrentPosition" :disabled="loading">Geolocate me</button>
    </div>
  </div>
</geolocation>
```

Usage
------


Sources:
* https://adamwathan.me/renderless-components-in-vuejs/
* https://vuejs.org/v2/guide/components-slots.html#Scoped-Slots
