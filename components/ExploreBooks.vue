<template>
    <v-card tile flat class="mx-0 mt-4" color="transparent">
        <v-list-item class="mx-0 my-4">
            <div class="text-content">Explore</div>
            <v-spacer></v-spacer>
            <a class="text-content text-decoration-none">See Web</a>
        </v-list-item>

        <v-card dark flat color="transparent" v-for="(item, i) in fullList" :key="i">
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-avatar size="100" class="my-2 mt-6" rounded>
                        <v-img src="book.png"></v-img>
                    </v-avatar>
                </v-col>

                <v-col cols="8" class="ma-0 pa-0">
                    <v-card-text>
                        <div class="text-content font-weight-bold" v-text="item.title"></div>
                        <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;" v-html="contentStrip(item.content)"></p>
                        <div>
                            <v-btn color="primary" class="ma-0" text small tile @click="sendReadLater(item)">
                                <v-icon left>mdi-bookmark-outline</v-icon>
                                <span class="text-caption">Later</span>
                            </v-btn>
                            <v-btn color="green" class="ma-0" text small tile @click="SendReadData(item)">
                                <v-icon left>mdi-book-open</v-icon>
                                <span class="text-caption">Read</span>
                            </v-btn>
                        </div>
                    </v-card-text>
                </v-col>
            </v-row>
        </v-card>

    </v-card>
  </template>
  
  <script>
    export default {
      name: 'ExploreBooks',
      data () {
        return {
          ReadLater: [],
          fullList: null,
          nextPagetoken: null,
          errorMessage: null,
        }
      },

      methods: {
        async getEntireList() {
            try {
                await this.$axios
                .get('https://www.googleapis.com/blogger/v3/blogs/2149664745604437619/posts?key=AIzaSyBsV5RS2HBVbo8DSGY6uHyCgggFraBfRQU')
                .then((res) => {
                    this.fullList = res.data.items
                    this.nextPagetoken = res.nextPageToken
                })
                .catch((err) => {
                    this.errorMessage = err
                })
            } catch (error) {
                // this.$sentry.captureException(new Error(error))
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