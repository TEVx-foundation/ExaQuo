<template>
  <v-row>
    <v-col class="text-center">
      <blockquote class="blockquote">
        This is a dynamic page. <br>
        <footer>
          <small>
            Argument Passed - {{ text }}
          </small>
        </footer>
      </blockquote>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'DynamicPage',
  head () {
      return {
        title: this.$route.params.id,
      }
  },
  data () {
      return {
        text: this.$route.params.id,
      }
  },
}
</script>
