<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat>
        <v-card-title class="headline font-weight-light">
          Testing out VueJS along with nuxt framework.
        </v-card-title>
        <v-card-text>
          <p>{{ twoWay }}</p>
          <v-text-field
            label="Two-way binding" v-model="twoWay"
          ></v-text-field>
          <br>
          <p>{{ swapCase }}</p>
          <v-text-field
            label="Computed Value" v-model="computed"
          ></v-text-field>
          <br>
          <CustomComponent :title="componentProp"></CustomComponent>
          <v-text-field
            label="Component prop" v-model="componentProp"
          ></v-text-field>
          <br>
          <p>
            <nuxt-link :to="{ name: 'dynamic-id', params: {id: customLink}}">
              A router link
            </nuxt-link>
          </p>
          <v-text-field
            label="Component prop" v-model="customLink"
          ></v-text-field>
          <br>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data () {
      return {
        twoWay: 'This is two-way binding, changing the value of one changes the other values as well.',
        computed: 'This is a computed data, this automatically changes the case of the text.',
        componentProp: 'Argument to component',
        customLink: 'Passing-data',
      }
  },
  computed: {
    swapCase() {
      return this.computed.toUpperCase()
    }
  },
}
</script>
