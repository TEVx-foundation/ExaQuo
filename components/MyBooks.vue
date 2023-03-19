<template>
    <v-card tile flat class="mx-0 mt-4" color="transparent">
        <v-list-item class="mx-0">
            <div class="text-content">My Books</div>
            <v-spacer></v-spacer>
            <a class="text-content text-decoration-none">See More</a>
        </v-list-item>

        <v-slide-group
            v-model="MyBooksSlide"
            class="pa-0"
            active-class="success"
            >
            <v-slide-item v-for="(item, i) in data" :key="i">
                <v-card
                class="ma-2 text-center"
                height="180"
                width="130"
                @click="BookBar(item)"
                >                    
                    <v-avatar size="100" class="my-2 mt-4 mx-4" rounded>
                        <v-img src="book.png"></v-img>
                    </v-avatar>
                    <div class="text-content font-weight-bold text-body-2" v-text="item.title"></div>

                </v-card>
            </v-slide-item>
        </v-slide-group>

        <v-bottom-sheet v-model="BookBarValue" v-if="BookBarValue">
            <v-card dark flat>
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-avatar size="100" class="my-2 mt-6" rounded>
                        <v-img src="book.png"></v-img>
                    </v-avatar>
                </v-col>

                <v-col cols="8" class="ma-0 pa-0">
                    <v-card-text>
                        <div class="text-content font-weight-bold" v-text="SelectedBook.title"></div>
                        <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;" v-html="contentStrip(SelectedBook.content)"></p>
                        <div>
                            <v-btn color="primary" class="ma-0" text small tile>
                                <v-icon left>mdi-close</v-icon>
                                <span class="text-caption">Remove</span>
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
        </v-bottom-sheet>

    </v-card>
</template>
  
  <script>
    export default {
      name: 'MyBooks',
      props: ['data'],
      data () {
        return {
          MyBooksSlide: null,
          SelectedBook: null,
          BookBarValue: false,
          // BooksList: [],
        }
      },

      methods: {
        BookBar(e) {
            this.SelectedBook = e
            this.BookBarValue = true
        },

        contentStrip(content) {
            const c = content.replace(/(<([^>]+)>)/gi, '')
            return (c.slice(0, 95) + '...')
        },

        /* getMyBooks() {
            if (process.browser) {
                if (localStorage.getItem("ExaQuoPersonalBooks") === null) {
                    localStorage.setItem("ExaQuoPersonalBooks", this.BooksList)
                } else {
                    this.BooksList = localStorage.getItem("ExaQuoPersonalBooks")
                }
            }
        },

        updateList(e) {
            alert("Data Changed")
            this.BooksList.push(e)
            localStorage.setItem("ExaQuoPersonalBooks", this.BooksList)
        } */
      }, 

      mounted() {
        // this.getMyBooks()
        // alert(this.data)
      },
    }
  </script>

<style scoped>
.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}
</style>