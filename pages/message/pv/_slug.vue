<template>
  <div>
    <div>
      <v-card
        max-width="600"
        class="mx-auto mt-16"
        v-for="f in friend"
      >
        <v-app-bar
          dark
          color="blue darken-3"
          @click="dialog5 = true"

        >
          <v-dialog
            v-model="dialog5"
            max-width="240px"
          >
            <v-card v-for="g in friend">
              <v-avatar
                class="ma-5"
                size="200"
              >
                <v-img :src=g.profile_picture></v-img>
              </v-avatar>

              <v-card-title style="color: cornflowerblue">
                {{ g.first_name }} {{ g.last_name }}
              </v-card-title>
              <v-card-subtitle>
                @{{ g.username }}
              </v-card-subtitle>
              <v-card-subtitle>
                <v-icon small>mdi-cellphone-basic</v-icon>
                {{ g.phone }}
              </v-card-subtitle>
              <v-card-title style="color: cornflowerblue">
                bio
              </v-card-title>
              <v-card-subtitle>
                {{ g.bio }}
              </v-card-subtitle>


              <v-card-actions>

              </v-card-actions>
            </v-card>
          </v-dialog>

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
              <v-btn v-if="messages.length > messagess.length" text color="blue" @click="messagess = messages">see
                more
              </v-btn>
              <div v-for="message in messagess">

                <v-card
                  style="border-radius: 70px 10px 70px 70px;margin-left: 180px;"
                  v-if="message.sender == slug"
                  color=#8fcbf2
                  width="400"
                  class="mt-2 mb-2"
                >
                  <v-card-subtitle v-if="message.reply != null">
                    <v-banner
                      style="border-radius: 70px ;margin-left: 50px"
                      color=#cfebfc
                      width="300px"
                      class="grey--text text--darken-3"
                      single-line
                    >
                      <v-icon
                        slot="icon"
                        color=primary
                        size="36"
                      >
                        mdi-arrow-left-bottom-bold
                      </v-icon>
                      replay to
                      <v-chip small class="ml-2">{{ message.replay_text }}</v-chip>
                    </v-banner>
                  </v-card-subtitle>
                  <v-card-title>
                    <v-avatar class="ml-3">
                      <v-img :src=message.sender_pic></v-img>
                    </v-avatar>
                    <h4 style="color: black; margin-left: 5px;">{{ message.sender_name }}</h4>

                    <v-card-subtitle>{{ message.date }}</v-card-subtitle>
                  </v-card-title>
                  <v-img :src=message.pic></v-img>

                  <v-card-subtitle style="margin-left: 60px; color: black">{{ message.text }}</v-card-subtitle>
                  <v-icon v-if="message.seened == true" color="green">mdi-check-all</v-icon>
                  <v-icon v-if="message.seened == false" color="primary">mdi-check</v-icon>
                  <v-btn class="ml-16 mb-2 " x-small icon right>
                    <v-icon @click="deleteMessage(message.id)">mdi-delete-outline</v-icon>
                  </v-btn>

                </v-card>
                <v-card
                  style="border-radius: 10px 70px 70px 70px;"
                  width="400"
                  class="mt-2 mb-2"
                  v-if="message.sender != slug"
                  color="grey darken-3"
                  dark
                >
                  <v-card-subtitle v-if="message.reply != null">
                    <v-banner
                      style="border-radius: 70px ;margin-right: 50px"
                      color=#b5b5b5
                      class="grey--text text--darken-3"
                    >
                      <v-icon
                        slot="icon"
                        color=#484e52
                        size="36"
                      >
                        mdi-arrow-left-bottom-bold
                      </v-icon>
                      replay to
                      <v-chip small class="ml-2">{{ message.replay_text }}</v-chip>
                    </v-banner>
                  </v-card-subtitle>

                  <v-card-title>
                    <v-avatar>
                      <v-img :src=message.sender_pic></v-img>
                    </v-avatar>
                    <h4 style="margin-left: 5px;">{{ message.sender_name }}</h4>

                    <v-card-subtitle style="color: dodgerblue">{{ message.date }}</v-card-subtitle>
                  </v-card-title>
                  <v-img :src=message.pic></v-img>

                  <v-card-subtitle style="margin-left: 60px">{{ message.text }}</v-card-subtitle>
                  <v-btn class="ml-15 mb-2 " x-small icon right>
                    <v-icon @click="LikeMessage(message)">mdi-heart</v-icon>
                  </v-btn>
                  <v-btn class=" float-right " x-small icon right>
                    <v-icon @click="setRep(message) ,dialog3 = true">mdi-arrow-left-bottom-bold</v-icon>
                  </v-btn>
                  <v-dialog
                    v-model="dialog3"
                    max-width="500px"
                  >
                    <v-card>
                      <v-card-title>
                        <span>replay</span>
                      </v-card-title>


                      <v-card-actions>
                        <v-text-field
                          class="ml-5 "
                          label="message"
                          v-model="rep_text"
                          outlined
                        ></v-text-field>
                        <v-btn fab small class="orange">
                          <v-icon @click=" replay() , dialog3 = false">mdi-send</v-icon>
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-dialog>
                </v-card>
              </div>
            </v-col>
          </v-row>
        </v-container>

      </v-card>


    </div>

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
        <v-btn fab small class="primary">
          <v-icon @click="send">mdi-send</v-icon>
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
      dialog5: false,
      rep_text: '',
      dialog3: false,
      data: [],
      name: '',
      pic: null,
      dialog: false,
      slug: this.$route.params.slug,
      id: this.$route.query.id,
      rep: '',
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
      messagess: '',


    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/account/pv/' + this.slug + '/' + this.id)
      .then(response => {
        this.messages = response
        this.messagess = this.messages.slice(-10)
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/api/user/' + this.id)
      .then(response => {
        this.friend = response
        console.log(response)
      });
  },

  methods: {
    setRep(message) {
      console.log(message.id)
      this.rep = message.id

    },
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    send() {
      const formData = new FormData()
      formData.append("sender", this.slug);
      formData.append("receiver", this.id);
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
          window.location.href = "http://127.0.0.1:3000/message/pv/" + this.slug + '/?id=' + this.id
        }).catch(response => {
        window.alert(response)
      })
    },
    replay() {
      this.$axios.$post('http://127.0.0.1:8000/api/message', {
        text: this.rep_text,
        sender: this.slug,
        receiver: this.id,
        reply: this.rep,

      })
        .then(response => {
          console.log(response)
          window.alert('replay sent')
          window.location.href = "http://127.0.0.1:3000/message/pv/" + this.slug + '/?id=' + this.id
        }).catch(response => {
        window.alert(response)
      })
    },

    deleteMessage(id) {
      this.$axios.$delete('http://127.0.0.1:8000/api/message/' + id)
        .then(response => {
          console.log(response)
          window.alert('message deleted')
          window.location.href = "http://127.0.0.1:3000/message/pv/" + this.slug + '/?id=' + this.id
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
