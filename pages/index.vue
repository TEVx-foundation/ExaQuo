<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat color="transparent">

        <v-card color="transparent" class="ma-0" tile flat>

          <v-list-item class="ma-0 pa-0">
            <v-card-text>
              <div class="text-content">Good Morning</div>
              <p class="text-body-1 text--primary text-content mt-1">
                Stories to your fingertips
              </p>
            </v-card-text>

            <v-spacer></v-spacer>

            <v-btn icon color="#f9784b" href="https://exaquo.blogspot.com/"><v-icon>mdi-web</v-icon></v-btn>
          </v-list-item>

            <center>
            <v-btn-toggle
              borderless class="mx-auto"
            >
              <v-btn value="left" class="px-4">
                <span class="-hidden-sm-and-down text-caption" color="#784f91">My List</span>

                <v-icon right color="#f9784b">
                  mdi-filter-variant
                </v-icon>
              </v-btn>

              <v-btn value="center" class="px-5">
                <span class="-hidden-sm-and-down text-caption">Saved</span>

                <v-icon right color="#f9784b">
                  mdi-book
                </v-icon>
              </v-btn>

              <v-btn value="right" class="px-4">
                <span class="-hidden-sm-and-down text-caption">To-Read</span>

                <v-icon right color="#f9784b">
                  mdi-moon-new
                </v-icon>
              </v-btn>
            </v-btn-toggle>
          </center>

          <MyBooks class="ma-0" :data="BooksList" v-if="BooksList.length !== 0" @ReadData="OpenBook"></MyBooks>
          <ExploreBooks class="ma-0" :data="BooksListID" @updateReadLater="updateList" @ReadData="OpenBook"></ExploreBooks>

        </v-card>

        <v-dialog
          v-model="CurrentBookOpen"
          fullscreen v-if="CurrentBookOpen"
          hide-overlay
          transition="dialog-bottom-transition"
          max-width="290"
        >
          <v-card class="pb-2 justify-center" justify="center" color="#784f91">
            <v-toolbar
              dark tile flat color="#784f91"
            >
              <v-btn
                icon
                dark
                @click="CurrentBookOpen = false"
              >
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-spacer></v-spacer>
              <v-toolbar-title flat class="text-content text-body-1">Book Detail</v-toolbar-title>
              <v-spacer></v-spacer>
                <v-btn
                  icon :href="CurrentBook.url"
                  dark
                >
                  <v-icon>mdi-launch</v-icon>
                </v-btn>
            </v-toolbar>

            <center>
              <v-avatar size="150" class="mx-auto my-14" rounded>
                  <v-img src="book.png"></v-img>
              </v-avatar>
            </center>

            <v-list-item class="mx-0 my-4 justify-center">

              <v-btn icon x-large class="transparent">
                <v-icon x-large>mdi-play-circle-outline</v-icon>
              </v-btn>

              <v-card-title class="text-center justify-center">
                <div class="text-content text-h6">{{ CurrentBook.title }}</div>
              </v-card-title>
            </v-list-item>


            <v-card tile flat class="ma-0 pt-2">

              <v-chip-group
                mandatory class="mx-4 mt-4"
                active-class="primary--text"
              >
                <v-chip
                  v-for="tag in CurrentBook.labels"
                  :key="tag"
                >
                  {{ tag }}
                </v-chip>
              </v-chip-group>

              <v-card-title class="text-content">Content</v-card-title>
              <v-card-text class="text-content text-body-2" v-html="contentStrip(CurrentBook.content)" style="line-height: 1.8;">
              </v-card-text>     
            </v-card>   

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
        BooksListID: [],
      }
  },

  computed: {
    //
  },

  methods: {
      updateList(e) {
        if (this.BooksListID.includes(e.id) === false) {
          this.BooksList.push(e)
          this.BooksListID.push(e.id)
          localStorage.setItem("PersonalReadList", JSON.stringify(this.BooksList))
          localStorage.setItem("PersonalReadListID", JSON.stringify(this.BooksListID))
        }
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
      this.BooksListID = JSON.parse(localStorage.getItem("PersonalReadListID") || '[]')
    }
}
</script>

<style scoped>
.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}
</style>
