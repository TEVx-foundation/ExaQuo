<template>
    <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat color="transparent">

        <v-card color="transparent" class="ma-0" tile flat>
            <v-list-item class="mx-0 px-0 mt-6">
                <v-text-field dense prepend-inner-icon="mdi-magnify" clearable
                 solo label="Search Books" autofocus v-model="search"
                ></v-text-field>
            </v-list-item>

            <v-list-item class="mx-0 mb-2">
            <div class="text-content">Explore Books</div>
            <v-spacer></v-spacer>
            <a class="text-content text-decoration-none" style="color: #f9784b;">See Web</a>
        </v-list-item>

        <section v-if="loadingIco">

            <center>
                <v-progress-circular
                indeterminate class="mx-auto my-8"
                color="primary"
                ></v-progress-circular>
            </center>
        </section>

        <section v-else>

            <v-card tile flat class="mx-auto my-8" color="transparent" v-if="fullList === []">
                <v-img src="empty-library.png" class="mx-auto" max-width="200" contain></v-img>
            </v-card>

            <v-card dark flat color="transparent" v-for="(item, i) in fullList" :key="i">
                <v-row class="ma-0 px-2 pt-2">

                    <v-col cols="4" class="ma-0 pa-0">
                        <v-avatar size="100" class="my-2 mt-6" rounded>
                            <v-img src="book-open.png"></v-img>
                        </v-avatar>
                    </v-col>

                    <v-col cols="8" class="ma-0 pa-0">
                        <v-card-text>
                            <div class="text-content font-weight-bold" v-text="item.title"></div>
                            <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;" v-html="contentStrip(item.content)"></p>
                            <div>
                                <v-btn color="primary" class="ma-0" text small tile>
                                    <v-icon left>mdi-bookmark-outline</v-icon>
                                    <span class="text-caption">Later</span>
                                </v-btn>

                                <v-btn color="green" class="ma-0" text small tile>
                                    <v-icon left>mdi-book-open</v-icon>
                                    <span class="text-caption">Read</span>
                                </v-btn>
                            </div>
                        </v-card-text>
                    </v-col>
                </v-row>
            </v-card>
        </section>

        <v-card tile flat height="50" color="transparent">
        </v-card>

        </v-card>

      </v-card>
    </v-col>
    </v-row>
  </template>
  
  <script>
  export default {
    name: 'SearchPage',
    data () {
      return {
          search: "",
          loadingIco: false,
          fullList: [],
          nextPagetoken: null,
          errorMessage: null,
      }
  },

  methods: {
        async getEntireList() {
            this.loadingIco = true
            try {
                await this.$axios
                .get('https://www.googleapis.com/blogger/v3/blogs/2149664745604437619/posts/search?key=AIzaSyBsV5RS2HBVbo8DSGY6uHyCgggFraBfRQU&q=' + this.search)
                .then((res) => {
                    this.fullList = res.data.items
                    this.nextPagetoken = res.nextPageToken
                    this.loadingIco = false
                })
                .catch((err) => {
                    this.errorMessage = err
                    this.loadingIco = false
                })
            } catch (error) {
                this.loadingIco = false
                // this.$sentry.captureException(new Error(error))
            }
        },

        contentStrip(content) {
            const c = content.replace(/(<([^>]+)>)/gi, '')
            return (c.slice(0, 95) + '...')
        },

  },

  watch: {
    search: function (val) {
        this.getEntireList()
    }
  },

  mounted() {
    this.getEntireList()
  }
}
  </script>