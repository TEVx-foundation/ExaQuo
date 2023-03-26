<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card tile flat color="transparent">

              <v-card class="ma-0" tile flat color="transparent">
                <v-toolbar
                  dark tile flat
                >
                  <v-toolbar-title color="transparent">TEVx Developers</v-toolbar-title>
                  <v-spacer></v-spacer>
                    <v-btn
                      icon
                      dark
                    >
                      <v-icon>mdi-launch</v-icon>
                    </v-btn>
                </v-toolbar>

                <v-card tile flat class="mx-auto pb-4">
                  
                  <v-card class="mx-auto"
                    max-width="374" tile flat
                  >

                  <v-card-title class="mt-4">
                    <v-avatar size="60">
                      <img alt="tevx" src="TEVx.png">
                    </v-avatar>
                    <p class="ml-3 mt-5 text-h6 font-weight-light">
                      Team Vertex Foundation
                    </p>
                  </v-card-title>

                  <v-card-text class="mx-auto">
                    <div class="mb-4 mt-n2 text-center text-overline">
                      Sykaa Platforms Authored ©
                    </div>
                    <div>We are few, yet a passionate who enjoy creating solutions to help people around the world. We are working to build a foundation where developers all around the world can collaborate and work together. Our works empower everyone in this world to share ideas, offer support and make a difference.</div>
                  </v-card-text>

                  <v-card-title class="headline font-weight-light mt-0">
                    Our Vision
                  </v-card-title>
                  <v-card-text>
                    <p>Keeping our mission in mind, we will bring a transformation in the life of tech enthusiasts and take the idea of education to another level. Nothing here is somebody else’s problem. If you roll up your sleeves, sometimes you’re able to make an impact where you may not have thought you could.</p>
                  </v-card-text>

                  <v-card-title class="headline font-weight-light mt-0">
                    Get in Touch
                  </v-card-title>
                  <v-card-text>
                    <p>If you have any questions or feedback regarding the application or would like to contact the developer, please feel free to leave a message at mail. The developer would be happy to hear from you and will try to respond as soon as possible.
                    Thank you for your support and for using our application.</p>
                    <v-icon>mdi-gmail</v-icon> <a href="mailto:teamvertex@gmail.com" class="text-decoration-none mx-2">teamvertex@gmail.com</a>
                  </v-card-text>
                  </v-card>

                  <v-card color="transparent" tile flat>
                    <v-img
                      max-width="100" class="mx-auto mt-4 mb-8 pb-8"
                      src="Sykaa-logo.png" contain
                    ></v-img>
                  </v-card>

                </v-card>

              </v-card>
          </v-card>
        </v-col>
      </v-row>
  </template>
  
  <script>
  export default {
    name: 'Developers'
  }
  </script>