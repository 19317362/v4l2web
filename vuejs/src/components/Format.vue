<template>
  <div>
    <h1>{{ msg }}</h1>
    <tr>
    <td>
	    <select v-model="format.format">
		<option v-for="o in formats" :selected="o.format === format.format">{{o.format}}</option>
	    </select>
	    {{ format.description }} {{ format.width }}x{{ format.height }}
    </td>
    </tr>
  </div>
</template>

<script>
import axios from "axios";

var serviceurl = 'http://192.168.11.3:9999'

export default {
  mounted() {
        axios({ method: "GET", "url": serviceurl+ "/capabilities"}).then(response => {
		this.msg = response.data.card;
        });
        axios({ method: "GET", "url": serviceurl+ "/formats"}).then(response => {
		this.formats = response.data;

		axios({ method: "GET", "url": serviceurl+ "/format"}).then(response => {
			this.format = response.data;
			this.formats.forEach( item => { 
				if (item.format === this.format.format ) {
					this.format.description = item.description
				} 
			} );
		});
        });
    },
  data () {
    return {
      msg: 'loading...',
      formats: [],
      format: "",
      controls: [],
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
