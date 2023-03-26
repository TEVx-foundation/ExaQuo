<template>
    <v-card tile flat class="mx-0 mt-4" color="transparent">

        <v-card tile flat class="mx-auto my-8" color="transparent" v-if="books.length === 0">
            <v-img src="empty-library.png" class="mx-auto" max-width="200" contain></v-img>
        </v-card>

        <v-card dark flat color="transparent" v-for="(item, i) in books" :key="i" class="mx-2">
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-badge
                        bordered
                        color="#f9784b"
                        icon="mdi-progress-clock"
                        overlap
                    >
                        <v-avatar size="100" class="my-2 mt-6" rounded>
                            <v-img src="book-open.png"></v-img>
                        </v-avatar>
                    </v-badge>

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

                                <span>
                                    <v-icon left>mdi-progress-clock</v-icon>
                                    <span class="text-caption">Read</span>
                                </span>


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
      name: 'ContinueBooks',
      props: ['books', 'later'],
      data () {
        return {
            ReadLater: [],
        }
      },

      methods: {
        contentStrip(content) {
            const c = content.replace(/(<([^>]+)>)/gi, '')
            return (c.slice(0, 95) + '...')
        },

        checkLater(id) {
            if (this.later.includes(id) === false) {
              return true
            } else {
              return false
            }
        },

        sendReadLater (item) {
          this.$emit('updateReadLater', item)
        },

        SendReadData (item) {
          this.$emit('ReadData', item)
        },

        toggleReadLater(e) {
            if (this.ReadLater.includes(e)) {
                this.ReadLater = this.ReadLater.filter((item) => item !== e)
            } else {
                this.ReadLater.push(e)
            }
        },
      }
    }
  </script>