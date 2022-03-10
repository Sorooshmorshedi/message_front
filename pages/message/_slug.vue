<template>
  <div>
    <v-row justify="center">
      <v-col
        :key=1
        cols="auto"
      >
        <v-card
          width="400"
          class="mx-auto mt-16"
        >
          <v-app-bar
            dark
            color="blue darken-2"
          >
            <v-avatar right class="mr-2">
              <v-img
                src="https://cdn.wccftech.com/wp-content/uploads/2018/02/Android-Messages.jpg">
              </v-img>
            </v-avatar>

            <v-toolbar-title>Chats</v-toolbar-title>

            <v-spacer></v-spacer>

          </v-app-bar>

          <v-container>
            <v-row dense>
              <v-col cols="12">
                <v-card
                  class="mx-auto ma-2"
                  color="#484e52"
                  dark
                  max-width="380"
                  v-for="account in this.accounts"
                  @click="gotoChats(account)"

                >
                  <v-card-title>
                    <v-avatar>
                      <v-img
                        lazy-src="https://play-lh.googleusercontent.com/fgt7dyhffQu9eHEYf1rfrL_xYupnY4bWa1A3PUt_7xXAi5Gi6LxW3SLMaPQwEH37JV4"
                        :src=account.profile_picture>
                      </v-img>
                    </v-avatar>
                    <span class="text-h6 font-weight-light pa-2">{{ account.first_name }} {{ account.last_name }}</span>
                    <v-row
                      align="center"
                      justify="end"
                    >
                      <v-icon class="mr-1">
                        mdi-eye
                      </v-icon>
                    </v-row>

                  </v-card-title>
                </v-card>
              </v-col>

            </v-row>
          </v-container>
        </v-card>
      </v-col>
      <v-col
        :key=2
        cols="auto"
      >
        <v-card
          width="300"
          class="mx-auto mt-16"
        >
          <v-app-bar
            dark
            color="blue darken-2"
          >
            <v-avatar right class="mr-2">
              <v-img
                src="https://res.cloudinary.com/familink/image/upload/f_auto/v1644226137/telegram_-_1024px.png">
              </v-img>
            </v-avatar>

            <v-toolbar-title>Groups</v-toolbar-title>

            <v-spacer></v-spacer>

          </v-app-bar>

          <v-container>
            <v-row dense>
              <v-col cols="12">
                <v-card
                  class="mx-auto ma-2"
                  color="#484e52"
                  dark
                  max-width="450"
                  v-for="group in this.groups"
                  @click="gotoGroup(group)"


                >
                  <v-card-title>
                    <v-avatar>
                      <v-img
                        lazy-src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR6yR0rNApUrTg4fHMRSE0CiMC5rEweK4Um_cprO-vAnLLgNAUHoB7D1cWItA3Fd2kK2gY&usqp=CAU"
                        :src=group.pic>
                      </v-img>
                    </v-avatar>
                    <span class="text-h6 font-weight-light pa-2">{{ group.name }}</span>
                    <v-row
                      align="center"
                      justify="end"
                    >
                      <v-icon class="mr-1">
                        mdi-eye
                      </v-icon>
                    </v-row>

                  </v-card-title>
                  <v-card-subtitle style="margin-left: 60px;">{{ group.about }}</v-card-subtitle>

                </v-card>
              </v-col>

            </v-row>
          </v-container>
        </v-card>
      </v-col>
      <v-col
        :key=3
        cols="auto"
      >
        <v-card
          width="300"
          class="mx-auto mt-16"
        >
          <v-app-bar
            dark
            color="blue darken-2"
          >
            <v-avatar right class="mr-2">
              <v-img
                src="https://static10.tgstat.ru/channels/_0/61/61814e9a6d466ed8bdbf59d6cf0410b5.jpg">
              </v-img>
            </v-avatar>

            <v-toolbar-title>Channels</v-toolbar-title>

            <v-spacer></v-spacer>

          </v-app-bar>

          <v-container>
            <v-row dense>
              <v-col cols="12">
                <v-card
                  class="mx-auto ma-2"
                  color="#484e52"
                  dark
                  max-width="450"
                  v-for="channel in this.channels"
                  @click="gotoChannel(channel)"

                >
                  <v-card-title>
                    <v-avatar>
                      <v-img
                        lazy-src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR6yR0rNApUrTg4fHMRSE0CiMC5rEweK4Um_cprO-vAnLLgNAUHoB7D1cWItA3Fd2kK2gY&usqp=CAU"
                        :src=channel.pic>
                      </v-img>
                    </v-avatar>
                    <span class="text-h6 font-weight-light pa-2">{{ channel.name }}</span>
                    <v-row
                      align="center"
                      justify="end"
                    >
                      <v-icon class="mr-1">
                        mdi-eye
                      </v-icon>
                    </v-row>

                  </v-card-title>
                  <v-card-subtitle style="margin-left: 60px;">{{ channel.about }}</v-card-subtitle>
                </v-card>
              </v-col>

            </v-row>
          </v-container>
        </v-card>
      </v-col>
    </v-row>


  </div>
</template>

<script>
export default {
  data() {
    return {
      data: null,
      message: '',
      pic: null,
      dialog: false,
      slug: this.$route.params.slug,
      account: null,
      groups: '',
      f_name: '',
      l_name: '',
      phone: '',
      channels: '',
      file: null,
      account_name: [],
      accounts: [],
      my_user: '',

    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/api/account/chats/' + this.slug )
      .then(response => {
        this.accounts = response
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/api/account/groups/' + this.slug)
      .then(response => {
        console.log(response)
        this.groups = response
      });
    this.$axios.$get('http://127.0.0.1:8000/api/account/channels/' + this.slug)
      .then(response => {
        console.log(response)
        this.channels = response
      });
  },

  methods: {
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    gotoChats(account) {
      window.location.href="/message/pv/" + this.slug + '/?id=' + account.id
    },
    gotoGroup(group) {
      window.location.href="/message/group/" + this.slug + '/?id=' + group.id
    },
    gotoChannel(channel) {
      window.location.href="/message/channel/" + this.slug + '/?id=' + channel.id
    },

  },


}
</script>
