<template>
  <div>
    <v-card
      v-for="acc in account"
      class="mt-15 mb-15 mx-auto"
      max-width="700"
    >
      <v-card
        dark
        flat
      >
        <nuxt-link :to="{ path: '/profile/edit/'+ slug }">
          <v-btn
            absolute
            bottom
            color="blue darken-3"
            right
            small
            fab
          >
            <v-icon>mdi-account-edit-outline</v-icon>
          </v-btn>
        </nuxt-link>
        <v-card-title class="mt-10 pa-3 grey darken-3 blue--text text--lighten-2">
          <h3 class="text-h6 font-weight-light text-center grow">
            {{ acc.username }}
          </h3>
          <v-avatar>
            <v-img
              src="https://img.bfmtv.com/c/630/420/ff8/1c9d1bb1341acbe4aafd97f5783c2.png"></v-img>
          </v-avatar>
        </v-card-title>
        <v-img
          lazy-src="https://play-lh.googleusercontent.com/fgt7dyhffQu9eHEYf1rfrL_xYupnY4bWa1A3PUt_7xXAi5Gi6LxW3SLMaPQwEH37JV4"
          :src=acc.profile_picture>
        </v-img>
      </v-card>
      <v-card-text class="py-0">
        <v-timeline
          align-top
          dense
        >
          <v-timeline-item
            color="primary"
            small
          >
            <v-row class="pt-1">
              <v-col cols="3">
                <strong style="color: skyblue">Account</strong>
              </v-col>
              <v-col>
                <strong>{{ acc.phone }}</strong>
                <div class="text-caption" style="color: skyblue">
                  {{ acc.username }}
                </div>
              </v-col>
            </v-row>
          </v-timeline-item>

          <v-timeline-item
            color="blue darken-1"
            small
          >
            <v-row class="pt-1">
              <v-col cols="3">
                <strong style="color: skyblue">about</strong>
              </v-col>
              <v-col>
                <strong>{{ acc.first_name }} {{ acc.last_name }}</strong>
              </v-col>
            </v-row>
          </v-timeline-item>

          <v-timeline-item
            color="blue darken-2"
            small
          >
            <v-row class="pt-1">
              <v-col cols="3">
                <strong style="color: skyblue">Bio</strong>
              </v-col>
              <v-col>
                <strong>{{ acc.bio }}</strong>
              </v-col>
            </v-row>
          </v-timeline-item>
        </v-timeline>
        <v-col>
          <nuxt-link :to="{ path: '/message/'+ $route.params.slug }">
            <v-btn color="primary" fab>
              <v-icon>mdi-inbox</v-icon>
            </v-btn>
          </nuxt-link>
        </v-col>


      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      slug: this.$route.params.slug,
      account: null,
      username: '',
      f_name: '',
      l_name: '',
      phone: '',
      bio: '',
      file: null,
    }
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
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    editAcc() {
      const formData = new FormData()
      formData.append("first_name", this.f_name);
      formData.append("username", this.username);
      formData.append("last_name", this.l_name);
      formData.append('profile_picture', this.file)
      formData.append("bio", this.bio);
      formData.append("phone", this.phone);
      this.data = formData
      console.log(this.data)
      this.$axios.$put('http://127.0.0.1:8000/account/' + this.user + '/', this.data)
        .then(response => {
          console.log(response)
          window.alert('account edited')
          window.location.href = "http://127.0.0.1:3000/account/" + response.id;
        }).catch(response => {
        window.alert('pick a image for your profile')
      })
    }


  },


}
</script>
