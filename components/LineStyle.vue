<template>

 <div class="my-4">
  <v-card class="overflow-auto mb-2" height="60vh" tile flat id="MainScroll" style="margin-top: 10vh;">
    <v-card tile flat class="spacer" align="center">
        <v-card-text class="text-content text-center justify-center text-h3">
            {{ title }}
        </v-card-text>
    </v-card>

    <div class="text mt-8">
      <div v-for="(line, index) in paragraphs" :key="index">
        <p v-if="ActiveIndex == index + 1" class="line text-content" id="ScrollIndex" ref="ScrollIndex">{{ line }}.</p>
        <p v-else>

            <p v-if="ActiveIndex == index" class="active-line text-content">{{ line }}.</p>
            <p v-else-if="ActiveIndex !== index + 1" class="line text-content">{{ line }}.</p>

        </p>
      </div>
    </div>

    <v-card tile flat class="spacer"></v-card>
  </v-card>

  <v-row
    v-touch="{
      left: () => swipe('Left'),
      right: () => swipe('Right'),
      up: () => NextLine(),
      down: () => PreviousLine(),
    }"
    align="center"
    justify="center"
    style="height: 20vh; width: 100vw;"
  >
    <v-icon x-large>mdi-gesture-swipe-vertical</v-icon>
  </v-row>

</div>
</template>
  
  <script>
    export default {
    name: 'LineStyle',
    props: ['paragraphs', 'title'],
    data () {
    return {
        swipeDirection: 'None',
        ActiveIndex: 0,
        options: {
            duration: 1000,
            offset: -150,
            easing: 'easeInOutCubic',
        },
     }
    },

    methods: {
        swipe (direction) {
            this.swipeDirection = direction
        },

        NextLine() {
            if (this.ActiveIndex < this.paragraphs.length - 1) {
                this.ActiveIndex++
                document.getElementById('ScrollIndex').scrollIntoView({behavior: 'smooth', inline: 'center'})
                document.getElementById('ScrollIndex').scrollTop = 150
            }
        },

        PreviousLine() {
            if (this.ActiveIndex > 0) {
                this.ActiveIndex--
                //$vuetify.goTo('#ScrollIndex', this.options)
                document.getElementById('ScrollIndex').scrollIntoView({ behavior: "smooth", block: "center", inline: "center" })
                // document.getElementById('ScrollIndex').scrollTop = 150
            }
        },
    },

    mounted() {
        //
    },

}
</script>

<style scoped>
.spacer {
  height: 30vh;
}

.line {
  opacity: 0.2;
  font-size: 1.1rem;
  text-align: center;
  margin: 1rem;
}

.active-line {
  font-size: 1.1rem;
  text-align: center;
  margin: 1rem;
}

.highlighted {
  opacity: 1;
}

.text-content {
  font-family: 'Montserrat', sans-serif !important; 
}
</style>