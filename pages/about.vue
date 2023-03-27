<template>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card tile flat color="transparent">
  
                <v-card class="ma-0" tile flat color="transparent">  
                  <v-card tile flat class="mx-auto pb-4" color="transparent">

                    <v-card-title class="text-h6 font-weight-light mt-0">
                        An Overview
                    </v-card-title>
                    
                    <v-card class="mx-auto" color="transparent"
                      max-width="374" tile flat
                    >
  
                    <v-card-title class="mt-2 justify-center">
                      <v-avatar size="100">
                        <img alt="tevx" src="app-logo-white.png">
                      </v-avatar>
                      <p class="mt-5 text-h5 font-weight-light text-content">
                        ExaQuo
                      </p>
                    </v-card-title>
  
                    <v-card-text class="mx-auto mb-0 pb-0">
                      <div class="mb-4 mt-n2 text-center text-overline">
                        Developed by
                      </div>

                      <v-card-title class="mt-0 pt-0 justify-center">
                        <v-avatar size="60">
                            <img alt="tevx" src="TEVx-transparent.png">
                        </v-avatar>
                        <p class="ml-3 mt-5 text-h6 font-weight-light">
                            TEVx Foundation
                        </p>
                      </v-card-title>
                    </v-card-text>

                    <v-card-text class="mx-auto mt-0 pt-0">
                      <div class="mb-4 mt-n2 text-center text-overline">
                        Authored by
                      </div>

                      <v-card color="transparent" tile flat class="mt-n4 pt-0">
                        <v-img
                            max-width="150" class="mx-auto mt-4 mb-8 pb-8"
                            src="Sykaa-ico-logo.png" contain
                        ></v-img>
                      </v-card>

                    </v-card-text>
  
                    <v-card-title class="headline font-weight-light mt-n8">
                      Our Vision
                    </v-card-title>
                    <v-card-text>
                      <p>ExaQuo is a service that searches for articles and short thrilling stories to the public. It is a versatile and user-friendly platform that caters to the needs of those who enjoy short stories.
                        ExaQuo offers a rich selection of short thrilling stories that are sure to captivate readers. This is a testing application implementation of the ExaQuo service. You can visit the main blog from here.
                      </p>
                      <v-icon>mdi-web</v-icon> <a href="https://exaquo.blogspot.com/" class="text-decoration-none mx-2">https://exaquo.blogspot.com/</a>
                    </v-card-text>
  
                    
                    <v-expansion-panels class="mt-8" tile>
                    <v-expansion-panel tile>
                        <v-expansion-panel-header>
                        Release Notes for 1.2 beta
                        </v-expansion-panel-header>
                        <v-expansion-panel-content>

                            <v-list-item two-line>
                            <v-list-item-content>
                                <v-list-item-title>Supports Offline Reading</v-list-item-title>
                                <v-card-text>Any book you've marked or Added to read later will be automatically cached and made available to you offline.</v-card-text>
                            </v-list-item-content>
                            </v-list-item>

                            <v-list-item two-line>
                            <v-list-item-content>
                                <v-list-item-title>Reading and non-read books</v-list-item-title>
                                <v-card-text>A new option is made available to easily see the list of books you've marked as reading as well the books which you've not read yet.</v-card-text>
                            </v-list-item-content>
                            </v-list-item>

                        </v-expansion-panel-content>
                    </v-expansion-panel>
                    </v-expansion-panels>

                    </v-card>
  
                    <v-card color="transparent" tile flat>
                      <v-img
                        max-width="100" class="mx-auto mt-4 mb-8 pb-8"
                        src="Sykaa-ico-logo.png" contain
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
      name: 'AboutPage'
    }
    </script>