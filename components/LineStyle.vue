<template>

 <div>
  <v-card class="overflow-auto mb-8" height="400" tile flat id="MainScroll">
    <v-card tile flat class="spacer"></v-card>

    <div class="text mt-8">
      <div v-for="(line, index) in paragraphs" :key="index">
        <p v-if="ActiveIndex == index + 1" class="line" id="ScrollIndex">{{ line }}.</p>
        <p v-else>

            <p v-if="ActiveIndex == index" class="active-line">{{ line }}.</p>
            <p v-else-if="ActiveIndex !== index + 1" class="line">{{ line }}.</p>

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
    style="height: 200px; width: 100vw;"
  >
    <v-icon x-large>mdi-gesture-swipe-vertical</v-icon>
  </v-row>

</div>

</template>
  
  <script>
    // import { ScrollTrigger } from "gsap/dist/ScrollTrigger";

    export default {
    name: 'LineStyle',
    props: ['paragraphs'],
    data () {
    return {
        swipeDirection: 'None',
        ActiveIndex: 0,
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
                document.getElementById('ScrollIndex').scrollIntoView({behavior: 'smooth', inline: 'center'})
                document.getElementById('ScrollIndex').scrollTop = 150
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
  height: 50vh;
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
</style>