<template>
  <v-app dark>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>GTS</span>
        <span class="font-weight-light">Converter</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <GTSConverter/>
    </v-content>
    <v-footer class="pa-3">
        <v-spacer></v-spacer>
        <div>
          Value calculations, some code & inspiration sourced from AbsentStar and Byl@http://www.giantessworld.net/convertor.html.
          &copy; Shadow Larkens {{ new Date().getFullYear() }}.
        </div>
      </v-footer>
  </v-app>
</template>

<script>
import GTSConverter from './components/GTSConverter'

export default {
  name: 'App',
  components: {
    GTSConverter
  },
  data () {
    return {
      //
    }
  }
}
</script>
