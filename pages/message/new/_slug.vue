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
            lazy-src="https://play-lh.googleusercontent.com/fgt7dyhffQu9eHEYf1rfrL_xYupnY4bWa1A3PUt_7xXAi5Gi6LxW3SLMaPQwEH37JV4"
            src="https://cdn.dribbble.com/users/1677926/screenshots/7058161/media/387786c663902ebd9a4cae4955ceb1a4.gif">
          </v-img>
        </v-avatar>

        <v-toolbar-title>new Message</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-message</v-icon>
        </v-btn>
      </v-app-bar>

      <v-container>
        <v-row dense>
          <v-col cols="12">
            <v-card
              color="grey darken-3"
              dark
            >
              <v-card-title class="text-h5">
                select a friend
              </v-card-title>

              <v-autocomplete
                class="ml-15 mr-15  "
                v-model="account"
                :items="accounts"
                item-text="item.id"
                item-value="id"
                deletable-chips
                filled
                chips
                rounded
              >
                <template v-slot:selection="data">
                  <v-chip
                    v-bind="data.attrs"
                    :input-value="data.selected"
                    close
                  >
                    <v-avatar left>
                      <v-img :src="data.item.pic"></v-img>
                    </v-avatar>
                    {{ data.item.name }} {{data.item.lname}}
                  </v-chip>
                </template>
                <template v-slot:item="data">
                  <template >
                    <v-list-item-avatar>
                      <img :src="data.item.pic">
                    </v-list-item-avatar>
                    <v-list-item-content>
                      <v-list-item-title v-html="data.item.name"></v-list-item-title>
                      <v-list-item-subtitle v-html="data.item.lname"></v-list-item-subtitle>
                    </v-list-item-content>
                  </template>
                </template>
              </v-autocomplete>
              <v-textarea
                class="blue--text ma-2"
                outlined
                name="input-7-4"
                label="Message"
                v-model="message"
                append-icon="mdi-comment"

              ></v-textarea>
              <v-card-text>
                <input id="image-upload" type="file" ref="file" @change="uploadFile"/>
              </v-card-text>


              <v-card-actions class="justify-end">
                <v-btn color="primary" @click="sendMessage" >
                  <v-icon>mdi-send</v-icon>
                </v-btn>
                <v-btn fab x-small class="primary">
                  <v-icon @click="dialog11 = true">mdi-clock</v-icon>
                </v-btn>
                <v-dialog
                  v-model="dialog11"
                  max-width="500px"
                >
                  <v-card class="pa-3">
                    <v-card-title>
                      <span>timing message</span>
                    </v-card-title>

                    <v-time-picker
                      v-model="time"
                      :landscape="$vuetify.breakpoint.mdAndUp"
                      full-width
                      type="month"
                    ></v-time-picker>
                    <v-date-picker v-model="picker" class="ml-16"></v-date-picker>
                    <v-card-actions>
                      <v-text-field
                        class="ml-5 "
                        label="message"
                        v-model="message"
                        outlined
                      ></v-text-field>
                      <v-btn fab small class="blue">
                        <v-icon @click=" timeSend() , dialog11 = false">mdi-send</v-icon>
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>

              </v-card-actions>

            </v-card>
          </v-col>

        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  data() {
    return {
      dialog11: false,
      picker:'',
      time: '10:10',
      datetime : new Date(),
      now: moment().format(),
      data: null,
      message: '',
      pic: null,
      dialog: false,
      slug: this.$route.params.slug,
      account: null,
      username: '',
      f_name: '',
      l_name: '',
      phone: '',
      bio: '',
      file: null,
      account_name : [],
      accounts: [],
      my_user: '',

    }
  },
  beforeMount() {
    this.$axios.$get('http://127.0.0.1:8000/api/user/' + this.slug)
      .then(response => {
        this.my_user = response
        console.log(response)
        console.log(this.my_user)
      })
  },

  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/user' )
      .then(response => {

        for (let ac in response) {
          this.accounts.push({
            'name': response[ac].first_name,
            'lname': response[ac].last_name,
            'id': response[ac].id,
            'pic': response[ac].profile_picture
          })
          this.account_name.push(response[ac].first_name)
        }
      })
    console.log(this.accounts)
  },

  methods: {
    timeSend() {
      console.log(this.picker)
      console.log(this.time.slice(3))
      this.datetime = new Date(this.picker)
      this.datetime.setHours(parseInt(this.time.slice(0,2)))
      this.datetime.setMinutes(parseInt(this.time.slice(3)))
      console.log(this.datetime)
      this.$axios.$post('http://127.0.0.1:8000/api/message', {
        text: this.message,
        sender: this.slug,
        receiver: this.account,
        date: this.datetime
      })
        .then(response => {
          console.log(response)
          window.alert('timer message activate')
          window.location.href = "http://127.0.0.1:3000/message/pv/" + this.slug + '/?id=' + this.account
        }).catch(response => {
        window.alert(response)
      })
    },

    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    sendMessage() {
      const formData = new FormData()
      formData.append("sender", this.slug);
      formData.append("receiver", this.account);
      formData.append("text", this.message);
      if (this.file != null) {formData.append('pic', this.file)}
      this.data = formData
      console.log(this.data)
      this.$axios.$post('http://127.0.0.1:8000/api/message', this.data)
        .then(response => {
          console.log(response)
          window.alert('message sent')
          window.location.href="http://127.0.0.1:3000/message/pv/" + this.slug + "/?id=" + this.account
        }).catch(response => {
        window.alert(response)
      })
    }
  },


}
</script>
