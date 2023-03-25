<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat color="transparent">

        <v-card color="transparent" class="ma-0" tile flat>

          <v-list-item class="ma-0 pa-0">
            <v-card-text>
              <div class="text-content">{{ greetings }}</div>
              <p class="text-body-1 text--primary text-content mt-1">
                Stories to your fingertips
              </p>
            </v-card-text>

            <v-spacer></v-spacer>

            <v-btn icon :color="AppColorPalatte" @click="AppColor = true"><v-icon>mdi-circle-edit-outline</v-icon></v-btn>
          </v-list-item>

            <center>
            <v-btn-toggle background-color="#252525"
              borderless class="mx-1 d-flex flex-row justify-space-between"
            >
              <v-btn value="left" class="px-4" width="33%" to="continue">
                <span class="-hidden-sm-and-down text-caption" color="#784f91">Continue</span>

                <v-icon right :color="AppColorPalatte">
                  mdi-filter-variant
                </v-icon>
              </v-btn>

              <v-spacer></v-spacer>

              <v-btn value="center" class="px-5" width="33%">
                <span class="-hidden-sm-and-down text-caption">Saved</span>

                <v-icon right :color="AppColorPalatte">
                  mdi-book
                </v-icon>
              </v-btn>

              <v-spacer></v-spacer>

              <v-btn value="right" class="px-4"  width="33%" to="toread">
                <span class="-hidden-sm-and-down text-caption">To-Read</span>

                <v-icon right :color="AppColorPalatte">
                  mdi-moon-new
                </v-icon>
              </v-btn>
            </v-btn-toggle>
          </center>

          <MyBooks class="ma-0" :data="BooksList" v-if="BooksList.length !== 0" @ReadData="OpenBook"></MyBooks>
          <ExploreBooks class="ma-0" :data="BooksListID" :continue="ContinueBookListID" :read="ReadBookListID" @updateReadLater="updateList" @ReadData="OpenBook"></ExploreBooks>

        </v-card>

        <v-dialog
          v-model="CurrentBookOpen"
          fullscreen v-if="CurrentBookOpen"
          hide-overlay
          transition="dialog-bottom-transition"
          max-width="290"
        >
          <v-card class="pb-2 justify-center" justify="center" :color="ReadColorPalatte">
            <v-toolbar
              dark tile flat :color="ReadColorPalatte"
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
                  icon @click="ReadingColor = true"
                  dark
                >
                  <v-icon>mdi-circle-edit-outline</v-icon>
                </v-btn>
            </v-toolbar>

            <center>
              <v-avatar size="250" class="mx-auto mt-14 mb-0" rounded>
                  <v-img src="book-open.png"></v-img>
              </v-avatar>
            </center>

            <v-list-item class="mx-0 mt-n10 mb-4 justify-center">

              <v-card-title class="text-center justify-center">
                <div class="text-content text-h6">{{ CurrentBook.title }}</div>
              </v-card-title>
            </v-list-item>

            <v-list-item class="pa-0 justify-center mb-4 pb-8">

              <v-btn-toggle background-color="transparent">
                <v-btn outlined class="text-content" style="text-transform: capitalize;">
                  <span class="font-weight-bold mr-2 mb-1">Eng</span>Language
                </v-btn>

                <v-btn outlined class="text-content" style="text-transform: capitalize;">
                  <span class="font-weight-bold mr-2 mb-1">{{ CurrentBookWords }}</span> Words
                </v-btn>
              </v-btn-toggle>

            </v-list-item>

            <v-list-item class="pa-0 justify-center mb-n6" style="z-index: 9;">

            <v-btn-toggle rounded style="border-width: 5px; color: white;">
              <v-btn outlined rounded ile class="text-content" style="text-transform: capitalize;" @click="ReadingModeSelect = true">
                <v-icon class="ml-1 mr-2" :color="ReadColorPalatte">mdi-filter-variant</v-icon>
                Read Book
              </v-btn>

              <v-btn outlined class="text-content" style="text-transform: capitalize;">
                <v-icon class="mr-2" :color="ReadColorPalatte">mdi-play-box</v-icon>
                Play Book
              </v-btn>
            </v-btn-toggle>

            </v-list-item>


            <v-card tile flat class="ma-0 pt-6">

              <v-chip-group
                mandatory class="mx-4 mt-10"
                active-class="primary--text"
              >
                <v-chip
                  v-for="tag in CurrentBook.labels"
                  :key="tag"
                >
                  {{ tag }}
                </v-chip>
              </v-chip-group>

              <v-card-title class="text-content">Content
                <v-spacer></v-spacer>
                <v-btn rounded text class="text-content force-captilize" @click="BookProgress = true"><v-icon class="mr-2">mdi-progress-check</v-icon>
                  <span v-if="CurrentBookStatus == -1">Mark</span>
                  <span v-if="CurrentBookStatus == 0">Continue</span>
                  <span v-if="CurrentBookStatus == 1">Finished</span>
                </v-btn>
              </v-card-title>
              <v-card-text class="text-content text-body-1 pb-8" v-html="contentStrip(CurrentBook.content)" style="line-height: 1.8;">
              </v-card-text>  

              <v-list-item class="ma-0 pa-2 pb-8">
                <v-btn icon>
                  <v-icon>mdi-circle</v-icon>
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn rounded outlined flat x-large class="ml-4" width="80%">
                  Next Book
                </v-btn>
                <v-spacer></v-spacer>
              </v-list-item>
              
              <v-overlay :value="SlideOverLay" opacity="1" style="z-index: 11;">

                <v-list-item class="py-0 my-0 mx-0 px-0" :color="ReadColorPalatte">
                  <v-spacer></v-spacer>
                  <v-btn icon @click="CloseSlide()"><v-icon>mdi-close</v-icon></v-btn>
                </v-list-item>
              
                <v-carousel
                  :continuous="false"
                  :show-arrows="false"
                  hide-delimiters
                  height="300"
                  v-model="SlideBookIndex"
                >
                  <v-carousel-item v-for="(item, i) in SlideBookList" :key="i">
                    <v-sheet :color="ReadColorPalatte"
                      height="100%"
                      tile width="100%"
                    >
                      <v-row
                        class="fill-height"
                        align="center"
                        justify="center"
                      >
                        <v-card class="text-body-1 ma-8 text-center" tile flat width="90vw" :color="ReadColorPalatte">
                          {{ item  }}.
                        </v-card>
                      </v-row>
                    </v-sheet>
                  </v-carousel-item>
                </v-carousel>

                <v-card tile flat class="mb-0 pb-0">
                <v-card-text>
                  <v-slider
                    step="1"
                    thumb-label
                    ticks v-model="SlideBookIndex"
                  ></v-slider>
                </v-card-text>
              </v-card>

              </v-overlay>

              <v-dialog fullscreen :value="LineOverlay" opacity="1" style="z-index: 11;">
                <v-card tile flat class="mb-0 pb-0" height="100%">

                  <v-list-item class="pa-0 ma-0 ml-2 pt-2 mb-8">
                    <v-btn icon @click="LineOverlay = false">
                      <v-spacer></v-spacer>
                      <v-icon>mdi-close</v-icon>
                    </v-btn>
                  </v-list-item>

                  <LineStyle :paragraphs="SlideBookList" :title="CurrentBook.title"></LineStyle>
                  
                </v-card>
              </v-dialog>

            </v-card>   

          </v-card>
        </v-dialog>

        <v-bottom-sheet v-model="ReadingModeSelect">
        <v-list shaped>
          <v-subheader class="text-content">Reading Mode</v-subheader>
          <v-list-item-group v-model="ReadingMode" color="primary">

            <v-list-item @click="ReadingModeSelect = false">
              <v-list-item-icon>
                <v-icon>mdi-format-paragraph-spacing</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Read as Paragraph</v-list-item-title>
            </v-list-item>
            <v-list-item @click="OpenSlide()">
              <v-list-item-icon>
                <v-icon>mdi-play-box-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Read as Slide</v-list-item-title>
            </v-list-item>
            <v-list-item @click="OpenLine()">
              <v-list-item-icon>
                <v-icon>mdi-format-line-weight</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Read as Lines</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-bottom-sheet>

      <v-bottom-sheet v-model="BookProgress">
        <v-list shaped>
          <v-subheader class="text-content">Select Progress</v-subheader>
          <v-list-item-group v-model="ProgressMode" color="primary">

            <v-list-item @click="AddtoContinueList()">
              <v-list-item-icon>
                <v-icon>mdi-progress-clock</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Mark as Reading</v-list-item-title>
            </v-list-item>
            <v-list-item @click="AddtoCompleteList()">
              <v-list-item-icon>
                <v-icon>mdi-check</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Mark as Complete</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-bottom-sheet>

      <v-bottom-sheet v-model="ReadingColor">
        <v-list shaped>
          <v-subheader class="text-content">Choose Color</v-subheader>
          <v-list-item-group v-model="ReadColorMode">

            <v-list-item @click="ChangeReadColor('#784f91')">
              <v-list-item-icon>
                <v-icon color="#784f91">mdi-circle</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Purple</v-list-item-title>
            </v-list-item>
            <v-list-item @click="ChangeReadColor('#f9784b')">
              <v-list-item-icon>
                <v-icon color="#f9784b">mdi-circle</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Orange</v-list-item-title>
            </v-list-item>
            <v-list-item @click="ChangeReadColor('#1e1e1e')">
              <v-list-item-icon>
                <v-icon color="white">mdi-circle-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Dark Grey</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-bottom-sheet>

      <v-bottom-sheet v-model="AppColor">
        <v-list shaped>
          <v-subheader class="text-content">Choose Theme Color</v-subheader>
          <v-list-item-group v-model="AppColorList">

            <v-list-item @click="ChangeAppColor('#784f91')">
              <v-list-item-icon>
                <v-icon color="#784f91">mdi-circle</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Purple</v-list-item-title>
            </v-list-item>
            <v-list-item @click="ChangeAppColor('#f9784b')">
              <v-list-item-icon>
                <v-icon color="#f9784b">mdi-circle</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Orange</v-list-item-title>
            </v-list-item>
            <v-list-item @click="ChangeAppColor('#1e1e1e')">
              <v-list-item-icon>
                <v-icon color="white">mdi-circle-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Dark Grey</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-bottom-sheet>

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
        greetings: "",
        CurrentBook: null,
        CurrentBookStatus: -1,
        CurrentBookWords: 0,
        SlideBookIndex: 0,

        ReadColorPalatte: "#784f91",

        AppColorPalatte: "#f9784b",
        ReadingColor: 0,
        AppColor: false,
        AppColorList: 1,
        BookProgress: false,

        CurrentBookOpen: false,
        SlideOverLay: false,
        LineOverlay: false,
        ReadingModeSelect: false,
        ReadingMode: 0,
        ProgressMode: 2,
        ReadColorMode: 0,

        ReadBookList: [],
        ReadBookListID: [],

        ContinueBookList: [],
        ContinueBookListID: [],

        SlideBookList: [],

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

      AddtoCompleteList() {
        if (this.ContinueBookListID.includes(this.CurrentBook.id) === false) {
          this.ContinueBookList = this.ContinueBookList.filter(el => el !== this.CurrentBook)
          this.ContinueBookListID = this.ContinueBookListID.filter(el => el !== this.CurrentBook.id)

          localStorage.setItem("PersonalContinueList", JSON.stringify(this.ContinueBookList))
          localStorage.setItem("PersonalContinueListID", JSON.stringify(this.ContinueBookListID))
        }

        if (this.ReadBookListID.includes(this.CurrentBook.id) === false) {
          this.ReadBookList.push(this.CurrentBook)
          this.ReadBookListID.push(this.CurrentBook.id)
          localStorage.setItem("PersonalCompleteList", JSON.stringify(this.ReadBookList))
          localStorage.setItem("PersonalCompleteListID", JSON.stringify(this.ReadBookListID))
        }
      },

      AddtoContinueList() {
        if (this.ReadBookListID.includes(this.CurrentBook.id) === true) {
          this.ReadBookList = this.ReadBookList.filter(el => el !== this.CurrentBook)
          this.ReadBookListID = this.ReadBookListID.filter(el => el !== this.CurrentBook.id)

          localStorage.setItem("PersonalCompleteList", JSON.stringify(this.ReadBookList))
          localStorage.setItem("PersonalCompleteListID", JSON.stringify(this.ReadBookListID))
        }

        if (this.ContinueBookListID.includes(this.CurrentBook.id) === false) {
          this.ContinueBookList.push(this.CurrentBook)
          this.ContinueBookListID.push(this.CurrentBook.id)
          localStorage.setItem("PersonalContinueList", JSON.stringify(this.ContinueBookList))
          localStorage.setItem("PersonalContinueListID", JSON.stringify(this.ContinueBookListID))
        }
      },

      checkStatus(e) {
        if (this.ReadBookListID.includes(e.id) === true) {
          this.CurrentBookStatus = 1
        } else if (this.ContinueBookListID.includes(e.id) === true) {
          this.CurrentBookStatus = 0
        } else {
          this.CurrentBookStatus = -1
        }
      },

      OpenBook(e) {
        this.CurrentBook = e
        this.CurrentBookOpen = true
        this.checkStatus(e)
      },

      contentStrip(content, e = 0) {
            var c = content.replace(/(<([^>]+)>)/gi, '')
            this.CurrentBookWords = c.split(" ").length
            const first = c.charAt(0)
            if (e == 0) {
              c = "<span style='font-size: 1.6rem; font-weight: 500;'>" + first + "</span>" + c.slice(1)
            }
            return (c)
      },

      OpenSlide() {
        this.ReadingModeSelect = false
        this.SlideOverLay = true
        this.ReadingMode = 0

        this.SlideBookList = this.contentStrip(this.CurrentBook.content, 1).split(".");
        this.SlideBookList = this.SlideBookList.filter(function (el) {
          return el != "";
        });

      },

      OpenLine() {
        this.ReadingModeSelect = false
        this.LineOverlay = true
        this.ReadingMode = 0

        this.SlideBookList = this.contentStrip(this.CurrentBook.content, 1).split(".");
        this.SlideBookList = this.SlideBookList.filter(function (el) {
          return el != "";
        });
      },

      CloseSlide() {
        this.SlideOverLay = false
        this.SlideBookList = []
      },

      ChangeReadColor(e) {
        this.ReadColorPalatte = e
      },

      ChangeAppColor(e) {
        this.AppColorPalatte = e
        this.$parent.$emit("AppColor", e)
      },

    },
  
    mounted() {
      var today = new Date()
      var curHr = today.getHours()

      if (curHr < 12) {
        this.greetings = "Good Morning"
      } else if (curHr < 18) {
        this.greetings = "Good Afternoon"
      } else {
        this.greetings = "Good Night"
      }

      this.BooksList = JSON.parse(localStorage.getItem("PersonalReadList") || '[]')
      this.BooksListID = JSON.parse(localStorage.getItem("PersonalReadListID") || '[]')

      this.ReadBookList = JSON.parse(localStorage.getItem("PersonalCompleteList") || '[]')
      this.ReadBookListID = JSON.parse(localStorage.getItem("PersonalCompleteListID") || '[]')

      this.ContinueBookList = JSON.parse(localStorage.getItem("PersonalContinueList") || '[]')
      this.ContinueBookListID = JSON.parse(localStorage.getItem("PersonalContinueListID") || '[]')
    }
}
</script>

<style scoped>
.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}

.force-captilize {
  text-transform: capitalize !important;
}
</style>
