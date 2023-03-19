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

          <MyBooks class="ma-0" :data="BooksList" v-if="BooksList !== null" @ReadData="OpenBook"></MyBooks>
          <ExploreBooks class="ma-0" @updateReadLater="updateList" @ReadData="OpenBook"></ExploreBooks>

        </v-card>

        <v-dialog
          v-model="CurrentBookOpen"
          fullscreen v-if="CurrentBookOpen"
          hide-overlay
          transition="dialog-bottom-transition"
          max-width="290"
        >
          <v-card class="pb-2 justify-center" justify="center">
            <v-toolbar
              dark tile flat
            >
              <v-btn
                icon
                dark
                @click="CurrentBookOpen = false"
              >
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title flat class="text-content text-body-1">{{ CurrentBook.title }}</v-toolbar-title>
              <v-spacer></v-spacer>
                <v-btn
                  icon
                  dark
                >
                  <v-icon>mdi-launch</v-icon>
                </v-btn>
            </v-toolbar>

            <center>
              <v-avatar size="150" class="mx-auto my-8" rounded>
                  <v-img src="book.png"></v-img>
              </v-avatar>
            </center>

            <v-card-text class="text-content text-body-2" v-html="contentStrip(CurrentBook.content)">
            </v-card-text>        

          </v-card>
        </v-dialog>

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
        CurrentBook: null,
        CurrentBookOpen: false,
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
      },

      OpenBook(e) {
        this.CurrentBook = e
        this.CurrentBookOpen = true
      },

      contentStrip(content) {
            const c = content.replace(/(<([^>]+)>)/gi, '')
            return (c)
      },
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
