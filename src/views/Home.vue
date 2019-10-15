<template>
<div class="bloc bgc-platinum l-bloc " id="bloc-1">
	<div class="container bloc-lg">
		<div class="row">
			<div class="col-sm-12">
				<div class="row">
					<div v-for="d in devices.result" v-bind:key="d.id+compKey" class="col-sm-4">
						<HelloWorld v-bind:device="d"/>
					</div>
        </div>
      </div>
    </div>
  </div>
  <button @click="refresh()" class="btn btn-dark-magenta btn-sq btn-m">Refresh</button>
</div>
</template>

<script>
// @ is an alias to /src
import { API, Auth } from 'aws-amplify'
import HelloWorld from '@/components/HelloWorld'

var apiName = 'CompanionAppAPI'
export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  data() {
    return {
      devices: [],
      userId: '',
      compKey: 0
    }
  },
  created:  function() {
    Auth.currentAuthenticatedUser()
      .then(async (user) => { 
        this.userId = user.username
        this.devices = await this.getDevices() 
      })
      .catch(() => { this.$router.push('/login')});
  },
  methods: {
    getDevices: async function() {
        console.log('Getting the devices')
        return await API.get(apiName, '/devices?user_id='+this.userId)
    },
    refresh: function() {
        this.getDevices().then(data => {console.log(data);this.devices=data; this.compKey += 1;}).catch(err=>{console.log(err)})
    }
  }
}
</script>
