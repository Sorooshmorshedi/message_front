<template>
  <div>
    <div>
      <v-row justify="center">
        <v-col
          :key=1
          cols="auto"
        >

          <v-card
            max-width="600"
            class="mx-auto mt-16"
            v-for="g in group"
          >
            <v-app-bar
              dark
              color="amber darken-2"
            >
              <v-avatar right class="mr-2">
                <v-img
                  lazy-src="https://play-lh.googleusercontent.com/fgt7dyhffQu9eHEYf1rfrL_xYupnY4bWa1A3PUt_7xXAi5Gi6LxW3SLMaPQwEH37JV4"
                  :src=g.pic>
                </v-img>
              </v-avatar>

              <v-toolbar-title>{{ g.name }}</v-toolbar-title>

              <v-spacer></v-spacer>
              <v-dialog
                v-model="dialog"
                persistent
                max-width="290"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="grey darken-3"
                    dark
                    v-bind="attrs"
                    v-on="on"
                  >
                    about
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title class="text-h7">
                    creator
                  </v-card-title>
                  <v-card-text>{{
                      g.creator_name
                    }}</v-card-text>
                  <v-card-title class="text-h7">
                    about
                  </v-card-title>
                  <v-card-text>{{
                      g.about
                    }}</v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="green darken-1"
                      text
                      @click="dialog = false"
                    >
                      close
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
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
                      <v-img :src=message.pic></v-img>

                      <v-card-subtitle style="margin-left: 60px; color: black">{{ message.text }}</v-card-subtitle>
                      <v-btn class="ma-2 " x-small icon right>
                        <v-icon @click="deleteMessage(message.id)">mdi-delete-outline</v-icon>
                      </v-btn>

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
                      <v-btn class="ma-2 " x-small icon right>
                        <v-icon @click="LikeMessage(message)">mdi-heart</v-icon>
                      </v-btn>
                    </v-card>
                  </div>
                </v-col>
              </v-row>
            </v-container>

          </v-card>
          <v-card
            width="600"
            class="mx-auto mt-1"
          >
            <v-card-subtitle>choose image</v-card-subtitle>
            <v-card-text>
              <input id="image-upload" type="file" ref="file" @change="uploadFile"/>
            </v-card-text>
            <v-card-actions>
              <v-text-field
                class="ma-2"
                label="message"
                v-model="text"
                outlined
              ></v-text-field>
              <v-btn fab small class="orange">
                <v-icon @click="send">mdi-send</v-icon>
              </v-btn>

            </v-card-actions>
          </v-card>
        </v-col>
        <v-col
          :key=2
          cols="auto"
        >
          <v-card
            max-width="300"
            class="mx-auto mt-16"
            v-for="g in group"
          >
            <v-app-bar
              dark
              color="amber darken-2"
            >

              <v-toolbar-title>members </v-toolbar-title>


            </v-app-bar>

            <v-container>
              <v-row dense>
                <v-col cols="12">
                  <div v-for="member in members">

                    <v-card
                      width="400"
                      class="mt-2 mb-2"
                      color="grey darken-3"
                      dark
                    >
                      <v-card-title>
                        <v-avatar size="30">
                          <v-img :src=member.profile_picture></v-img>
                        </v-avatar>
                        <h4 style="margin-left: 5px;">{{ member.first_name }} {{ member.last_name }}</h4>

                      </v-card-title>
                    </v-card>
                  </div>
                </v-col>
              </v-row>
            </v-container>

          </v-card>
        </v-col>
      </v-row>


    </div>

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
      id: this.$route.query.id,

      account: null,
      username: '',
      f_name: '',
      l_name: '',
      phone: '',
      about: '',
      file: null,
      account_name: [],
      accounts: [],
      my_user: '',
      messages: '',
      friend: '',
      text: '',
      group: '',
      members:'',


    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/group/chats/' + this.id)
      .then(response => {
        this.messages = response
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/api/group/' + this.id)
      .then(response => {
        this.group = response
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/api/group/members/' + this.id)
      .then(response => {
        this.members = response
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
      const formData = new FormData()
      formData.append("sender", this.slug);
      formData.append("group", this.id);
      formData.append("text", this.text);
      if (this.file != null) {
        formData.append('pic', this.file)
      }
      this.data = formData
      console.log(this.data)
      this.$axios.$post('http://127.0.0.1:8000/api/message', this.data)
        .then(response => {
          console.log(response)
          window.alert('message sent')
          window.location.href = "http://127.0.0.1:3000/message/group/" + this.slug + '/?id=' + this.id
        }).catch(response => {
        window.alert(response)
      })
    },
    deleteMessage(id) {
      this.$axios.$delete('http://127.0.0.1:8000/api/message/' + id)
        .then(response => {
          console.log(response)
          window.alert('message deleted')
          window.location.href = "http://127.0.0.1:3000/message/group/" + this.slug + '/?id=' + this.id
        }).catch(response => {
        window.alert(response)
      })
    },
    LikeMessage(message) {
      this.$axios.$post('http://127.0.0.1:8000/api/like', {
        message: message.id,
        user: this.slug
      })
        .then(response => {
          console.log(response)
          window.alert('message liked')
        }).catch(response => {
        this.$axios.$get('http://127.0.0.1:8000/api/unlike/' + this.slug + '/' + message.id).then(response => {
          console.log(response)
          window.alert('message unliked')
        })
      })
    },

  },


}
</script>
