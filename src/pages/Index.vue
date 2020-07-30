<template>
  <q-page class="q-pa-none fit">
    <!-- <keep-alive> -->
      <pizzas :numItems.sync="numitems" :rightdrawer.sync="rd"></pizzas>
    <!-- </keep-alive> -->
  </q-page>
</template>

<script>
import pizzas from 'components/pizzas.vue'

export default {
  name: 'PageIndex',
  components:{ pizzas },

  props:['nofitems','right'],

  computed: {
    rd: {
      get(){return this.right},
      set(rd){this.$emit('update:right', rd)}
    },
    numitems: {
      get(){return this.nofitems},
      set(numitems){this.$emit('update:nofitems', numitems)}
    }
  },

}
</script>
