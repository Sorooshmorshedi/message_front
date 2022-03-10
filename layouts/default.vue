<template>
  <v-app dark>
    <v-toolbar
      dark
      prominent
      src="https://www.triangleprideband.com/wp-content/uploads/2021/06/blue-gradient-background-1.png"
    >
      <v-app-bar-nav-icon
        v-if="$route.params.slug != undefined"
      ></v-app-bar-nav-icon>

      <v-toolbar-title class="withe--text text--darken-2">Messaging app</v-toolbar-title>

      <v-spacer></v-spacer>
      <nuxt-link :to="{ path: '/profile/'+ $route.params.slug }">
        <v-btn
          icon
          v-if="$route.params.slug != undefined"
        >
          <v-icon color="withe">mdi-account</v-icon>
        </v-btn>
      </nuxt-link>
      <NuxtLink to="/">
        <v-btn
          icon
          v-if="$route.params.slug != undefined"
        >
          <v-icon color="withe">mdi-export</v-icon>
        </v-btn>
      </NuxtLink>


    </v-toolbar>
    <v-main>
      <v-container>
        <Nuxt/>
      </v-container>
    </v-main>
    <v-footer
      dark
      padless
    >
      <v-card
        class="flex"
        flat
        tile
      >
        <v-card-title class="blue lighten-2">
          <strong class="subheading grey--text text--darken-3">Get connected with Designer & devoloper on social
            networks!</strong>
          <v-spacer></v-spacer>
          <v-btn
            target="_blank"
            href="https://www.instagram.com/devoloper_soroosh/"
            class="mx-4"
            color="grey darken-3"
            icon
          >
            <v-icon size="24px">
              mdi-instagram
            </v-icon>
          </v-btn>
          <v-btn
            target="_blank"
            href="https://api.whatsapp.com/send/?phone=989035894088&text&app_absent=0"
            class="mx-4"
            color="grey darken-3"
            icon
          >
            <v-icon size="24px">
              mdi-whatsapp
            </v-icon>
          </v-btn>
          <v-btn
            target="_blank"
            href="https://gitlab.com/sorooshmorshedi"
            color="grey darken-3"
            class="mx-4"
            icon
          >
            <v-icon size="24px">
              mdi-gitlab
            </v-icon>
          </v-btn>

          <v-btn
            target="_blank"
            href="https://github.com/Sorooshmorshedi"
            color="grey darken-3"
            class="mx-4"
            icon
          >
            <v-icon size="24px">
              mdi-github
            </v-icon>
          </v-btn>


          <v-btn
            target="_blank"
            href="https://www.linkedin.com/in/sorooshmorshedi/"
            class="mx-4"
            color="grey darken-3"

            icon
          >
            <v-icon>
              mdi-linkedin
            </v-icon>
          </v-btn>

        </v-card-title>

        <v-card-text class="py-2 grey darken-3 text-center">
          {{ new Date().getFullYear() }}/{{ new Date().getMonth() }}/{{ new Date().getUTCDate() }} — <strong>Soroosh
          morshedi</strong>
        </v-card-text>
      </v-card>
    </v-footer>

  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {}
  }
}
</script>
