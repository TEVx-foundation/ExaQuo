<template>
    <v-card tile flat class="mx-0 mt-4" color="transparent">
        <v-list-item class="mx-0">
            <div class="text-content">My Books</div>
            <v-spacer></v-spacer>
            <a class="text-content text-decoration-none" style="color: #f9784b;" @click="AllBooks = true">See More</a>
        </v-list-item>

        <v-card tile flat class="mx-auto my-8" color="transparent" v-if="data.length === 0">
            <v-img src="empty-library.png" class="mx-auto" max-width="200" contain></v-img>
        </v-card>

        <v-slide-group
            v-model="MyBooksSlide"
            class="pa-0"
            active-class="success"
            >
            <v-slide-item v-for="(item, i) in data.slice(0, 5)" :key="i">

            <v-card tile flat height="180" width="130" class="ma-4 pa-0" color="red" opacity="0.5">

                <v-card
                class="ma-2 text-center"
                height="180"
                width="130"
                @click="BookBar(item)"
                >                    
                    <v-avatar size="80" class="my-2 mt-6 mx-4" rounded>
                        <v-img src="book-open.png"></v-img>
                    </v-avatar>
                    <div class="text-content font-weight-bold text-body-2 mx-2" v-text="item.title"></div>

                </v-card>

            </v-card>

                <!-- v-img src="https://source.unsplash.com/random/390x540/?night" width="130" height="180" class="ma-2 text-center">
                    <div class="text-content font-weight-bold text-body-2 mx-2" v-text="item.title"></div>
                </v-img -->

            </v-slide-item>
        </v-slide-group>

        <v-bottom-sheet v-model="BookBarValue" v-if="BookBarValue">
            <v-card dark flat>
            <v-row class="ma-0 px-2 pt-2">

                <v-col cols="4" class="ma-0 pa-0">
                    <v-avatar size="100" class="my-2 mt-6 mx-2" rounded>
                        <v-img src="book-open.png"></v-img>
                    </v-avatar>
                </v-col>

                <v-col cols="8" class="ma-0 pa-0">
                    <v-card-text>
                        <div class="text-content font-weight-bold" v-text="SelectedBook.title"></div>
                        <p class="text-caption text--primary text-content mt-1 -text-truncate" style="line-clamp: 2;" v-html="contentStrip(SelectedBook.content)"></p>
                        <div>
                            <v-btn color="primary" class="ma-0" text small tile @click="RemoveBook(SelectedBook)">
                                <v-icon left>mdi-close</v-icon>
                                <span class="text-caption">Remove</span>
                            </v-btn>
                            <v-btn color="green" class="ma-0" text small tile @click="SendReadData(SelectedBook)">
                                <v-icon left>mdi-book-open</v-icon>
                                <span class="text-caption">Read</span>
                            </v-btn>
                        </div>
                    </v-card-text>
                </v-col>

            </v-row>
        </v-card>
        </v-bottom-sheet>

        <v-bottom-sheet v-model="AllBooks">
            <v-card flat tileclass="ma-0 pa-0 overflow-auto">
                <v-list>
                    <v-subheader class="text-content text-body-1">Read Later</v-subheader>

                    <v-list-item class="mx-2 px-0 mt-4">
                        <v-text-field dense prepend-inner-icon="mdi-magnify" clearable color="blue"
                        solo label="Search Books" autofocus v-model="search"
                        ></v-text-field>
                    </v-list-item>

                    <v-list-item
                    v-for="(item, i) in data"
                    :key="i"
                    @click="BookBar(item)"
                    >
                    <v-list-item-avatar>
                        <v-avatar size="32px" tile >
                        <img src="book-open.png">
                        </v-avatar>
                    </v-list-item-avatar>
                    <v-list-item-title class="text-content text-body-1">{{ item.title }}</v-list-item-title>
                    </v-list-item>
                </v-list>
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
          AllBooks: false,
          search: null,
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

        RemoveBook(e) {
            // var Manipulated = JSON.parse(localStorage.getItem("PersonalReadListID") || '[]')

            var Manipulated = JSON.parse(localStorage.getItem("PersonalReadListID") || '[]')
            Manipulated = Manipulated.filter(el => el !== e.id)

            this.data = this.data.filter(el => el !== e)
            localStorage.setItem("PersonalReadList", JSON.stringify(this.data))
            localStorage.setItem("PersonalReadListID", JSON.stringify(Manipulated))

            this.BookBarValue = false
        },

        SendReadData(e) {
            this.$emit('ReadData', e)
        },
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