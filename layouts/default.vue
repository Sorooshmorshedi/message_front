<template>
  <v-app dark>
    <v-toolbar
      dark
      prominent
      src="https://www.triangleprideband.com/wp-content/uploads/2021/06/blue-gradient-background-1.png"
    >
      <v-app-bar-nav-icon
        @click.stop="drawer = !drawer"
        v-if="$route.params.slug != undefined"
      ></v-app-bar-nav-icon>
      <v-toolbar-title
        style="font-size: 50px; font-family: Cursive  ;"
        class="withe--text text--darken-2"

      >Messaging app
      </v-toolbar-title>

      <v-spacer></v-spacer>
      <v-btn
        @click="logout"
        icon
        v-if="$route.params.slug != undefined"
      >
        <v-icon color="withe">mdi-export</v-icon>
      </v-btn>


    </v-toolbar>
    <v-navigation-drawer
      v-model="drawer"
      absolute
      bottom
      temporary
    >
      <v-list
        nav
        dense
      >
        <v-list-item-group
          v-model="group"
          active-class="blue--text text--blue-4"
        >
          <v-list-item v-for="a in account">
            <v-list-item-avatar>
              <v-img :src=a.profile_picture></v-img>
            </v-list-item-avatar>
            <v-list-item-title>{{ a.username }}</v-list-item-title>
            <v-list-item-subtitle class="text-h9 primary--text font-weight-light">online</v-list-item-subtitle>
          </v-list-item>
          <v-list-item
            @click="goPro"
            v-for="a in account" link>
            <v-list-item-icon>
              <v-icon>mdi-account</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-subtitle class="text-h8">
                {{ a.first_name }} {{ a.last_name }}
              </v-list-item-subtitle>
              <v-list-item-subtitle>{{ a.phone }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item
            @click="goInbox"
          >
            <v-list-item-icon>
              <v-icon>mdi-inbox</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7">inbox</v-list-item-title>
          </v-list-item>

          <v-list-item
            @click="newG"
          >
            <v-list-item-icon>
              <v-icon>mdi-account-group</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7">new group</v-list-item-title>
          </v-list-item>

          <v-list-item
            @click="newC"
          >
            <v-list-item-icon>
              <v-icon>mdi-bullhorn</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7">new channel</v-list-item-title>
          </v-list-item>
          <v-list-item
            @click="newM"
          >
            <v-list-item-icon>
              <v-icon>mdi-message-plus</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7">new Message</v-list-item-title>
          </v-list-item>

          <v-list-item
            @click="Archived"
          >
            <v-list-item-icon>
              <v-icon>mdi-bookmark</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="text-h7">Archive</v-list-item-title>
          </v-list-item>

        </v-list-item-group>

      </v-list>

    </v-navigation-drawer>
    <v-main>
      <v-card>
        <v-container>
          <Nuxt/>
        </v-container>
      </v-card>


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
  data() {
    return {
      drawer: false,
      group: null,
      account: '',
      slug: this.$route.params.slug,
    }
  },
  watch: {
    group() {
      this.drawer = false
    },
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/user/' + this.slug)
      .then(response => {
        this.account = response
        console.log(response)
        console.log(this.account)
      })
  },
  methods: {
    goInbox() {
      window.location.href = "http://127.0.0.1:3000/message/" + this.slug;
    },
    goPro() {
      window.location.href = "http://127.0.0.1:3000/profile/" + this.slug;
    },
    newG() {
      window.location.href = "http://127.0.0.1:3000/message/group/create/" + this.slug;
    },

    newC() {
      window.location.href = "http://127.0.0.1:3000/message/channel/create/" + this.slug;
    },
    newM() {
      window.location.href = "http://127.0.0.1:3000/message/new/" + this.slug;
    },
    Archived() {
      window.location.href = "http://127.0.0.1:3000/message/archived/" + this.slug;
    },
    logout() {
      this.$axios.$get('http://127.0.0.1:8000/api/logout')
        .then(response => {
          console.log(response)
          window.location.href = "http://127.0.0.1:3000/"
        })
    }


  }
}
</script>
