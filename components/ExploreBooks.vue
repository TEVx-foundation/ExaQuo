<template>
    <v-card tile flat class="mx-0 mt-4" color="transparent">
        <v-list-item class="mx-0 my-4 pr-0">
            <div class="text-content">Explore</div>
            <v-spacer></v-spacer>
            <v-btn icon style="color: #f9784b;" @click="OpenWeb = true" class="mr-0 pr-0">
                <v-icon>mdi-web</v-icon>
            </v-btn>

            <!-- a class="text-content text-decoration-none" style="color: #f9784b;" @click="OpenWeb = true">See Web</a -->

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
        <v-card dark flat color="transparent" v-for="(item, i) in fullList" :key="i">
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-badge v-if="checkStatus(item) == 0"
                        bordered
                        color="#f9784b"
                        icon="mdi-progress-clock"
                        overlap
                    >
                        <v-avatar size="100" class="my-2 mt-6" rounded>
                            <v-img src="book-open.png"></v-img>
                        </v-avatar>
                    </v-badge>

                    <v-avatar size="100" class="my-2 mt-6" rounded v-else>
                        <v-img src="book-open.png"></v-img>
                    </v-avatar>

                </v-col>

                <v-col cols="8" class="ma-0 pa-0">
                    <v-card-text>
                        <div class="text-content font-weight-bold" v-text="item.title"></div>
                        <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;" v-html="contentStrip(item.content)"></p>
                        <div>
                            <v-btn color="primary" class="ma-0" text small tile v-if="checkLater(item.id)" @click="sendReadLater(item)">
                                <v-icon left>mdi-bookmark-outline</v-icon>
                                <span class="text-caption">Later</span>
                            </v-btn>

                            <v-btn color="primary" class="ma-0" text small tile v-else>
                                <v-icon left>mdi-check-all</v-icon>
                                <span class="text-caption">Added</span>
                            </v-btn>

                            <v-btn color="green" class="ma-0" text small tile @click="SendReadData(item)">

                                <span v-if="checkStatus(item) == -1">
                                    <v-icon left>mdi-book-open</v-icon>
                                    <span class="text-caption">Read</span>
                                </span>

                                <span v-else-if="checkStatus(item) == 0">
                                    <v-icon left>mdi-progress-clock</v-icon>
                                    <span class="text-caption">Read</span>
                                </span>

                                <span v-else>
                                    <v-icon left>mdi-check-all</v-icon>
                                    <span class="text-caption">Done</span>
                                </span>


                            </v-btn>
                        </div>
                    </v-card-text>
                </v-col>
            </v-row>
        </v-card>


        <v-list-item class="ma-0 pa-2 pt-8 pb-4" v-if="nextPagetoken !== null || nextPagetoken !== undefined">
        <v-btn icon>
            <v-icon>mdi-circle</v-icon>
        </v-btn>
        <v-spacer></v-spacer>
        <v-btn rounded outlined large class="ml-4" width="80%" @click="getNextList()" :loading="nextLoading">
            Load More
        </v-btn>
        <v-spacer></v-spacer>
        </v-list-item>

        </section>

        <v-bottom-sheet v-model="OpenWeb">
            <v-card dark flat>
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-avatar size="100" class="my-2 mt-6 mx-2" rounded>
                        <v-img src="blogspot.png"></v-img>
                    </v-avatar>
                </v-col>

                <v-col cols="8" class="ma-0 pa-0">
                    <v-card-text>
                        <div class="text-content font-weight-bold">Open in Web</div>
                        <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;">
                          Clicking on continue will redirect you to the main webpage of ExaQuo.
                        </p>
                        <div>
                            <v-btn color="primary" class="ma-0" text small tile @click="OpenWeb = false">
                                <v-icon left>mdi-close</v-icon>
                                <span class="text-caption">Close</span>
                            </v-btn>
                            <v-btn color="green" class="ma-0" text small tile href="https://exaquo.blogspot.com/">
                                <v-icon left>mdi-web</v-icon>
                                <span class="text-caption">Visit</span>
                            </v-btn>
                        </div>
                    </v-card-text>
                </v-col>

            </v-row>
        </v-card>
        </v-bottom-sheet>

        <v-card tile flat height="50" color="transparent">
        </v-card>

    </v-card>
  </template>
  
  <script>
    export default {
      name: 'ExploreBooks',
      props: ['data', 'continue', 'read'],
      data () {
        return {
          ReadLater: [],
          fullList: null,
          nextList: null,

          nextPagetoken: null,
          errorMessage: null,
          loadingIco: true,
          nextLoading: false,

          OpenWeb: false,
          mainLink: 'https://www.googleapis.com/blogger/v3/blogs/2149664745604437619/posts?key=AIzaSyBsV5RS2HBVbo8DSGY6uHyCgggFraBfRQU',
        }
      },

      methods: {
        async getEntireList() {
            this.loadingIco = true
            try {
                await this.$axios
                .get('https://www.googleapis.com/blogger/v3/blogs/2149664745604437619/posts?key=AIzaSyBsV5RS2HBVbo8DSGY6uHyCgggFraBfRQU')
                .then((res) => {
                    this.fullList = res.data.items
                    this.nextPagetoken = res.data.nextPageToken
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

        async getNextList() {
            this.nextLoading = true
            try {
                await this.$axios
                .get('https://www.googleapis.com/blogger/v3/blogs/2149664745604437619/posts?key=AIzaSyBsV5RS2HBVbo8DSGY6uHyCgggFraBfRQU&pageToken=' + this.nextPagetoken)
                .then((res) => {
                    this.nextList = res.data.items
                    this.nextPagetoken = res.data.nextPageToken
                    this.fullList = this.fullList.concat(this.nextList)

                    console.log(this.nextList)

                    this.nextLoading = false
                })
                .catch((err) => {
                    this.errorMessage = err
                    this.nextLoading = false
                })
            } catch (error) {
                this.nextLoading = false
                // this.$sentry.captureException(new Error(error))
            }
        },

        checkLater(id) {
            if (this.data.includes(id) === false) {
              return true
            } else {
              return false
            }
        },

        checkStatus(e) {
            if (this.read.includes(e.id) === true) {
                return 1
            } else if (this.continue.includes(e.id) === true) {
                return 0
            } else {
                return -1
            }
        },

        contentStrip(content) {
            const c = content.replace(/(<([^>]+)>)/gi, '')
            return (c.slice(0, 95) + '...')
        },

        sendReadLater(e) {
            this.$emit('updateReadLater', e)
        },

        SendReadData(e) {
            this.$emit('ReadData', e)
        },

        toggleReadLater(e) {
            if (this.ReadLater.includes(e)) {
                this.ReadLater = this.ReadLater.filter((item) => item !== e)
            } else {
                this.ReadLater.push(e)
            }
        }
    },

    mounted() {
        this.getEntireList()
    },

    }
  </script>

<style scoped>
.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}
</style>