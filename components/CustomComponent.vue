<template>
  <v-card tile flat class="grey darken-3 ma-4">
  <p class="text-h6 font-weight-light">Hello from a component ! Argument passed - {{ title }}</p>
  </v-card>
</template>

<script>
  export default {
    name: 'CustomComponent',
    props: ['title'],
    data () {
      return {
        text: 'Prop passed to a component.'
      }
    },
  }
</script>