<template>
  <q-layout view="hHh LpR fFf">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="left = !left" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg">
          </q-avatar>
          Pizza Love
        </q-toolbar-title>

        <q-btn dense flat round icon="mdi-cart" @click="right = !right">
          <q-badge color="green" floating>{{ nitems }}</q-badge>
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="left" side="left" bordered>
      <!-- drawer content -->
    </q-drawer>

    <!-- <q-drawer v-model="right" @click="rd=!rd" side="right" bordered> -->
      <!-- drawer content -->
    <!-- </q-drawer> -->

    <q-page-container>
      <router-view :nofitems.sync="nitems" :right.sync="right"/>
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  data () {
    return {
      left: false,
      right: false,
      nitems: 0,
      // rd:false,
    }
  },
  
}
</script>