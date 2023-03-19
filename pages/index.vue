<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat color="transparent">

        <v-card color="transparent" class="ma-0" tile flat>

          <v-card-text>
            <div class="text-content">Good Morning</div>
            <p class="text-body-1 text--primary text-content mt-1">
              ExaQuo stories to your fingertips
            </p>
          </v-card-text>

            <center>
            <v-btn-toggle
              borderless class="mx-auto"
            >
              <v-btn value="left" class="px-4">
                <span class="-hidden-sm-and-down text-caption">My List</span>

                <v-icon right>
                  mdi-filter-variant
                </v-icon>
              </v-btn>

              <v-btn value="center" class="px-5">
                <span class="-hidden-sm-and-down text-caption">Saved</span>

                <v-icon right>
                  mdi-book
                </v-icon>
              </v-btn>

              <v-btn value="right" class="px-4">
                <span class="-hidden-sm-and-down text-caption">To-Read</span>

                <v-icon right>
                  mdi-moon-new
                </v-icon>
              </v-btn>
            </v-btn-toggle>
          </center>

          <MyBooks class="ma-0" :data="BooksList" v-if="BooksList !== null"></MyBooks>
          <ExploreBooks class="ma-0" @updateReadLater="updateList"></ExploreBooks>

        </v-card>



        <!-- v-card-title class="headline font-weight-light">
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
        </v-card-text -->

      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data () {
      return {
        category: null,
        BooksList: [],
      }
  },

  computed: {
    //
  },

  methods: {
    updateList(e) {
        this.BooksList.push(e)
        localStorage.setItem("PersonalReadList", JSON.stringify(this.BooksList))
      }
    },
  
    mounted() {
      this.BooksList = JSON.parse(localStorage.getItem("PersonalReadList") || '[]')
    }
}
</script>

<style scoped>
.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}
</style>
