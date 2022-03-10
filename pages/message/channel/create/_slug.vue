<template>
  <div>
    <v-card
      max-width="400"
      class="mx-auto mt-16"
    >
      <v-app-bar
        dark
        color="blue darken-2"
      >
        <v-avatar right class="mr-2">
          <v-img
            src="https://cdn.dribbble.com/users/986000/screenshots/3420497/07-04-2017_1.gif">
          </v-img>
        </v-avatar>

        <v-toolbar-title>new Channel</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-link</v-icon>
        </v-btn>
      </v-app-bar>

      <v-container>
        <v-row dense>
          <v-col cols="12">
            <v-card
              color="grey darken-3"
              dark
            >
              <v-textarea
                class="pt-3 pr-3 pl-3 "
                label="Channel name"
                auto-grow
                outlined
                v-model="name"
                rows="1"
                row-height="15"
              ></v-textarea>
              <v-card-subtitle>Channel image</v-card-subtitle>
              <v-card-text>
                <input id="image-upload" type="file" ref="file" @change="uploadFile"/>
              </v-card-text>
              <v-textarea
                class="ma-2"
                outlined
                name="input-7-4"
                label="about channel"
                v-model="about"

              ></v-textarea>
              <v-switch
                class="ml-5"
                v-model="private"
                label="private"
                color="orange"
                hide-details
              ></v-switch>

              <v-textarea
                v-if="private == false"
                class="pt-3 pr-6 pl-6 "
                label="link"
                outlined
                v-model="link"
                rows="1"
                row-height="13"
              ></v-textarea>



              <v-card-actions class="justify-end">
                <v-btn color="primary" @click="CreateChannel" >
                  <v-icon>mdi-plus</v-icon>
                </v-btn>
              </v-card-actions>

            </v-card>
          </v-col>

        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      link: '',
      private: false,
      data: [],
      name: '',
      pic: null,
      dialog: false,
      slug: this.$route.params.slug,
      account: null,
      username: '',
      f_name: '',
      l_name: '',
      phone: '',
      about: '',
      file: null,
      account_name : [],
      accounts: [],
      my_user: '',

    }
  },

  methods: {
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    CreateChannel() {
      const formData = new FormData()
      formData.append("name", this.name);
      formData.append("creator", this.$route.params.slug);
      formData.append("joined_users", this.$route.params.slug);
      formData.append("private", this.private);
      if (this.private == false){formData.append('link', this.link)}
      if (this.file != null){formData.append('pic', this.file)}
      formData.append("about", this.about);
      this.data = formData
      console.log(this.data)
      this.$axios.$post('http://127.0.0.1:8000/api/channel', this.data)
        .then(response => {
          console.log(response)
          window.alert('Channel created')
          window.location.href = "http://127.0.0.1:3000/message/channel/" + this.slug + '/?id=' + response.id;
        }).catch(response => {
        window.alert(response)
      })
    }
  },


}
</script>






















