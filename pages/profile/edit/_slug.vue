<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card
        class="mx-auto mt-16 mb-16"
      >
        <v-toolbar
          flat
          color=orange
        >
          <v-icon>mdi-account</v-icon>
          <v-toolbar-title class="font-weight-light">
            Profile edit
          </v-toolbar-title>
        </v-toolbar>

        <v-card-text>
          <v-card-title> profile picture</v-card-title>
          <input id="image-upload" type="file" ref="file" @change="uploadFile"/>
        </v-card-text>

        <v-card-text>
          <v-text-field
            v-model="username"
            color="white"
            label="user name"
          ></v-text-field>
        </v-card-text>
        <v-card-text>

          <v-text-field
            v-model="f_name"
            color="white"
            label="First name"
          ></v-text-field>
        </v-card-text>
        <v-card-text>

          <v-text-field
            v-model="l_name"
            color="white"
            label="Last name"
          ></v-text-field>
        </v-card-text>
        <v-card-text>
          <v-text-field
            v-model="phone"
            color="white"
            label="phone number"
          ></v-text-field>
        </v-card-text>
        <v-card-text>

          <v-text-field
            v-model="bio"
            color="white"
            label="bio"
          ></v-text-field>
        </v-card-text>


        <v-card-actions>
          <v-spacer></v-spacer>
          <nuxt-link :to="{ path: '/profile/'+ user }">
            <v-btn
              color='orange'
            >
              cancell
            </v-btn>
          </nuxt-link>
          <v-btn
            color='green darken-2'
            @click="editAcc">
            edit account
          </v-btn>


        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>

</template>


<script>
export default {
  name: 'create account',
  data() {
    return {
      show1: false,
      password: '',
      rules: {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters',
      },
      user: this.$route.params.slug,
      f_name: '',
      l_name: '',
      phone: '',
      bio: '',
      data: '',
      file: null,
      username: '',


    }
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
      formData.append("phone", this.phone);
      formData.append('profile_picture', this.file)
      formData.append("bio", this.bio);
      this.data = formData
      console.log(this.data)
      this.$axios.$put('http://127.0.0.1:8000/api/user/edit/' + this.user + '/', this.data)
        .then(response => {
          console.log(response)
          window.alert('account edited')
          window.location.href = "http://127.0.0.1:3000/profile/" + response.id;
        }).catch(response => {
        window.alert('pick a image for your profile')
      })
    }
  }
}
</script>
<style>

</style>
