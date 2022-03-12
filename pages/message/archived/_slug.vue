<template>
  <div>
    <div>
      <v-card
        max-width="600"
        class="mx-auto mt-16"
      >
        <v-app-bar
          dark
          color="blue darken-3"

        >
          archived
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
                  <v-card-title>
                    <v-avatar class="ml-3">
                      <v-img :src=message.sender_pic></v-img>
                    </v-avatar>
                    <h4 style="color: black; margin-left: 5px;">{{ message.sender_name }}</h4>
                    <h6 class="ml-5 blue--text" v-if="message.receiver">to {{message.reciver_name}}</h6>
                    <h6 class="ml-5 blue--text" v-if="message.channel">to channel {{message.channel_name}}</h6>
                    <h6 class="ml-5 blue--text" v-if="message.group">to group {{message.group_name}}</h6>
                    <v-icon @click="unarchive(message)" class="ml-2">mdi-bookmark-minus</v-icon>

                    <v-card-subtitle>{{ message.date }}</v-card-subtitle>
                  </v-card-title>
                  <v-img :src=message.pic></v-img>

                  <v-card-subtitle style="margin-left: 60px; color: black">{{ message.text }}</v-card-subtitle>
                </v-card>
                <v-card
                  style="border-radius: 10px 70px 70px 70px;"
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
                    <h6 class="ml-5 blue--text" v-if="message.receiver">to {{message.reciver_name}}</h6>
                    <h6 class="ml-5 blue--text" v-if="message.channel">to channel {{message.channel_name}}</h6>
                    <h6 class="ml-5 blue--text" v-if="message.group">to group {{message.group_name}}</h6>
                    <v-icon @click="unarchive(message)" class="ml-2">mdi-bookmark-minus</v-icon>

                    <v-card-subtitle style="color: dodgerblue">{{ message.date }}</v-card-subtitle>
                  </v-card-title>
                  <v-img :src=message.pic></v-img>

                  <v-card-subtitle style="margin-left: 60px">{{ message.text }}</v-card-subtitle>
                </v-card>
              </div>
            </v-col>
          </v-row>
        </v-container>

      </v-card>


    </div>


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
    this.$axios.$get('http://127.0.0.1:8000/api/account/archived/' + this.slug )
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
    saveMessage(message) {
      this.$axios.$post('http://127.0.0.1:8000/api/archive', {
        user: this.slug,
        message: message.id
      })
        .then(response => {
          console.log(response)
          window.alert('archived')
        }).catch(response => {
        window.alert('you arechived this')
      })
    },
    unarchive(message) {
      this.$axios.$get('http://127.0.0.1:8000/api/unarchive/' + message.id + '/' + this.slug)
        .then(response => {
          console.log(response)
          window.alert('unarchived')
          window.location.href='http://127.0.0.1:3000/message/archived/'+ this.slug
        })
    },


  },


}
</script>
