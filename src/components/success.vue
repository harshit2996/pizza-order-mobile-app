<template>
  <div class="text-center">
    
    <q-dialog
      v-model="dialog"
      hide-overlay
      persistent
      width="300"

    >
        <q-card class="q-ma-0 q-pa-0">
          <div>
            <q-img
              contain
              height="128"
              src="https://pizzalove.herokuapp.com/img/logo.png"
            ></q-img>

          </div>
          <q-card-section>
            <div class justify="center" class="title text-center font-weight-light success--text" v-text="msg"></div>
          </q-card-section>
          <q-separator class="q-my-0"></q-separator>
          <q-card-actions class="q-pa-0 q-ma-0">
            <div>
              <div class="row" justify="end">
                <q-btn text type="a" :href="loc">
                  Return
                </q-btn>
              </div>
            </div>
          </q-card-actions>
        </q-card>
    </q-dialog>
  </div>
</template>

<script>

  import axios from 'axios'

  export default {
    props:['success','message', 'location','order_id'],
    
    data () {
      return {
        // dialog: false,
      }
    },

    computed: {
      dialog: {
        get() { return this.success },
        set(dialog) {this.$emit('update:success', dialog)}
      },

      msg: {
        get() { return this.message },
        set(msg) {this.$emit('update:message', msg)}
      },
      loc: {
        get() { return this.location },
        set(loc) {this.$emit('update:location', loc)}
      },
      id: {
        get() { return this.order_id },
        set(id) {this.$emit('update:order_id', id)}
      },
        
    },
  }
</script>