<template>
  <div class="aside-content">
    <div>
    <p><img src="icons/Legend.png" class="fullwidth"><div class="caption">* these features appear when you zoom in</p></div>

    <p style="font-size:120%;"><strong><em>This map shows the major petroleum infrastructures of the United States and the world. From wellheads, platforms, ports and railway loaders, crude oil is piped to refineries that produce gas, diesel, kerosene, jet fuel, asphalt, petcoke and vast quantities of CO2.</em></strong></p>

    <p>For information about any of the elements, click on a line or a marker. The locations of most pipelines are approximate, but other features such as refineries are  accurate. When you zoom in close, a "spotlight" opens, allowing you to see what's on the ground.</p>

    <p>The buttons at lower left activate similar maps of coal and natural gas installations. The view is initially centered on the US, but you can hit the buttom at the bottom (center) to see the whole world. As it stands today, everywhere is Petropolis.</p>

<br>
<Accordion :open="false">
  <h3 slot="header">sources</h3>
  <div class="flex">
    <p>Coming...</p>
  </div>
</Accordion>

  </div>
</Accordion>

  </div>
</Accordion>

  </div>
</template>
<style scoped>
.flex {
  display: flex;
  flex-direction: column;
}
.flex div {
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}
.flex img {
  margin-right: 0.5em;
}
</style>
<script>
import AppLightBox from '../../components/AppLightBox.vue'
import Accordion from '../../components/Accordion.vue'

import {eventBus} from '../../main'

export default {
  name: 'PetropolisOil',
  components: {
    Accordion,
    AppLightBox
  },
  methods: {
    emit: function (method, args) {
      eventBus.$emit(method, args)
    }
  }
}
</script>
