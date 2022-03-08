<template>
  <div>
    <v-card
      max-width="400"
      class="mx-auto mt-16"
    >
      <v-app-bar
        dark
        color="amber darken-2"
      >
        <v-avatar right class="mr-2">
          <v-img
            src="https://cdn.dribbble.com/users/218750/screenshots/1310912/pennene.gif">
          </v-img>
        </v-avatar>

        <v-toolbar-title>edit Channel</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn
          v-for="c in channel"
          v-if="c.creator == slug"
          color="red darken-2"
          class="ma-2"
          dark
          @click="dialog = true"
        >
          delete
        </v-btn>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <v-card>
            <v-card-title>
              <span>sure you want delete?</span>
            </v-card-title>
            <v-card-subtitle>all the message and information of this channel will be deleted...</v-card-subtitle>
            <v-card-actions >
              <v-btn @click="DeleteChannel" text color="red">delete</v-btn>
              <v-btn text color="withe">cancell</v-btn>

            </v-card-actions>
          </v-card>
        </v-dialog>

      </v-app-bar>

      <v-container>
        <v-row dense>
          <v-col cols="12">
            <v-card
              color="grey darken-3"
              dark
            >
              <v-textarea
                class="pt-3 pr-3 pl-3 orange--text"
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
                class="orange--text ma-2"
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
                class="pt-3 pr-6 pl-6 orange--text"
                label="link"
                outlined
                v-model="link"
                rows="1"
                row-height="13"
              ></v-textarea>



              <v-card-actions class="justify-end">
                <v-btn color="orange" @click="CreateChannel" >
                  <v-icon>mdi-pencil</v-icon>
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
      id: this.$route.query.id,
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
      channel:'',

    }
  },
  beforeMount() {
    this.$axios.$get('http://127.0.0.1:8000/api/channel/' + this.id)
      .then(response => {
        this.channel = response
        console.log(this.channel[0].creator)
      });

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
      this.$axios.$put('http://127.0.0.1:8000/api/channel/' + this.id + '/', this.data)
        .then(response => {
          console.log(response)
          window.alert('Channel edit')
          window.location.href = "http://127.0.0.1:3000/message/channel/" + this.slug + '/?id=' + response.id
        }).catch(response => {
        window.alert(response)
      })
    },
    DeleteChannel() {
      this.$axios.$delete('http://127.0.0.1:8000/api/channel/' + this.id + '/')
        .then(response => {
          console.log(response)
          window.alert('Channel deleted')
          window.location.href = "http://127.0.0.1:3000/message/" + this.slug
        }).catch(response => {
        window.alert(response)
      })
    }

  },


}
</script>






















