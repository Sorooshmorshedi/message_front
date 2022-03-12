<template>
  <div>
    <v-card
      max-width="400"
      class="mx-auto "
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

        <v-toolbar-title>new Group</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-account-supervisor-circle</v-icon>
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
                label="Group name"
                auto-grow
                outlined
                v-model="name"
                rows="1"
                row-height="15"
              ></v-textarea>
              <v-card-title class="text-h5">
                add friends
              </v-card-title>

              <v-autocomplete
                class="ml-2 mr-2 "
                v-model="account"
                :items="accounts"
                item-text="item.id"
                item-value="id"
                deletable-chips
                filled
                chips
                multiple
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
                class=" ma-2"
                outlined
                name="input-7-4"
                label="about gruop"
                v-model="about"

              ></v-textarea>
              <v-switch
                class="ml-5"
                v-model="private"
                label="private"
                color="primary"
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
                <v-btn color="primary" @click="creatGroup" >
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
      data: null,
      private: false,
      link: null,
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
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    creatGroup() {
      this.$axios.$post('http://127.0.0.1:8000/api/group', {
        name:this.name,
        creator: this.slug,
        link: this.link,
        private: this.private,
        about: this.about,
        members: this.account})
        .then(response => {
          console.log(response)
          window.alert('group created')
          window.location.href="http://127.0.0.1:3000/profile/" + this.slug
        }).catch(response => {
        window.alert(response)
      })
    }
  },


}
</script>
