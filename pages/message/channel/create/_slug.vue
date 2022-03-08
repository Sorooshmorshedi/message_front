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


              <v-card-actions class="justify-end">
                <v-btn color="orange" @click="CreateChannel" >
                  <v-icon>mdi-account-multiple-plus-outline</v-icon>
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
      if (this.file != null){formData.append('pic', this.file)}
      formData.append("about", this.about);
      this.data = formData
      console.log(this.data)
      this.$axios.$post('http://127.0.0.1:8000/api/channel', this.data)
        .then(response => {
          console.log(response)
          window.alert('Channel created')
          window.location.href = "http://127.0.0.1:3000/profile/" + this.slug;
        }).catch(response => {
        window.alert(response)
      })
    }
  },


}
</script>

















<template>
  <div>
    <v-card
      max-width="600"
      class="mx-auto mt-16"
      v-for="f in friend"
    >
      <v-app-bar
        dark
        color="amber darken-2"
      >
        <v-avatar right class="mr-2">
          <v-img
            lazy-src="https://play-lh.googleusercontent.com/fgt7dyhffQu9eHEYf1rfrL_xYupnY4bWa1A3PUt_7xXAi5Gi6LxW3SLMaPQwEH37JV4"
            :src=f.profile_picture>
          </v-img>
        </v-avatar>

        <v-toolbar-title>{{ f.first_name }} {{ f.last_name }}</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-message</v-icon>
        </v-btn>
      </v-app-bar>

      <v-container>
        <v-row dense>
          <v-col cols="12">
            <div v-for="message in messages">

              <v-card
                v-if="message.sender == slug"
                color="yellow darken-1"
                width="400"

                class="mt-2 mb-2"
                style="margin-left: 180px;"
              >
                <v-card-title>
                  <v-avatar>
                    <v-img :src=message.sender_pic></v-img>
                  </v-avatar>
                  <h4 style="color: black; margin-left: 5px;">{{ message.sender_name }}</h4>

                  <v-card-subtitle>{{ message.date }}</v-card-subtitle>
                </v-card-title>
                <v-card-subtitle style="margin-left: 60px; color: black">{{ message.text }}</v-card-subtitle>
              </v-card>
              <v-card
                width="400"
                class="mt-2 mb-2"

                v-if="message.sender != slug"

                color="grey darken-3"
                dark
              >
                <v-card-title>
                  <v-avatar>
                    <v-img :src=message.sender_pic></v-img>
                  </v-avatar>
                  <h4 style="margin-left: 5px;">{{ message.sender_name }}</h4>

                  <v-card-subtitle style="color: gold">{{ message.date }}</v-card-subtitle>
                </v-card-title>
                <v-card-subtitle style="margin-left: 60px">{{ message.text }}</v-card-subtitle>
                <v-btn class="ma-2 " x-small icon right><v-icon>mdi-heart</v-icon></v-btn>
              </v-card>
            </div>
          </v-col>
        </v-row>
      </v-container>
      <v-card-actions>
        <v-text-field
          class="ma-2"
          label="message"
          v-model="text"
          outlined
        ></v-text-field>
        <v-btn @click="send" fab small class="orange"><v-icon>mdi-send</v-icon></v-btn>
      </v-card-actions>
    </v-card>


  </div>
</template>

<script>
export default {
  data() {
    return {
      text:'',
      data: null,
      message: '',
      pic: null,
      dialog: false,
      slug: this.$route.params.slug,
      id: this.$route.query.id,
      account: null,
      groups: '',
      f_name: '',
      l_name: '',
      phone: '',
      channels: '',
      file: null,
      account_name: [],
      messages: '',
      friend: '',

    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/account/pv/' + this.slug + '/' + this.id)
      .then(response => {
        this.messages = response
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/api/user/' + this.id)
      .then(response => {
        this.friend = response
        console.log(response)
      });

  },

  methods: {
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    send() {
      window.location.href
    }
  },


}
</script>





