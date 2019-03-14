<template>
  <div class="user_reference_input_component">
    <h2>Current search: {{current_search}}</h2>
    <h1>User Reference</h1><br>
    <textarea v-model="input_text" rows="5" cols="50" ref="text_element" placeholder="Text..." @input="inputText($event)"></textarea>
    <!--<div v-html="input_text"></div>-->
    <ul class="users_list" v-show="show_list">
      <li><h3>Utilisateurs</h3></li>
      <li class="single_user" v-for="user of users" v-show="!user.hidden" :key="user.id">
        <a href="#" class="single_user_link" @click="addUserToReference(user)">{{`@${user.nick_name} (${user.first_name + " " + user.last_name})`}}</a>
      </li>
    </ul>
    <div>
      <h3>Références:</h3>
      {{references}}
    </div>
  </div>
</template>

<script>
  import users from '../users'

  export default {
    data () {
      return {
        input_text: '',
        show_list: false,
        users: users,
        references: [],
        current_search: '',
        in_ref_mode: false,
        in_edit_mode: false
      }
    },
    methods: {
      getInputCursorPosition () {
        return this.$refs.text_element.selectionStart
      },
      toggleSearchOff () {
        this.show_list = false
        this.current_search = ''

        this.users.map(element => element.hidden = false)
      },
      searchList (stringToSearch) {
        console.log('SEARCH', stringToSearch)
        for (const user of users) {
          if ((user.first_name.toLowerCase().includes(stringToSearch) || user.last_name.toLowerCase().includes(stringToSearch) || user.nick_name.toLowerCase().includes(stringToSearch)) /*&& !this.references.some(element => element.user_ref === user.nick_name)*/) {
            user.hidden = false
          } else {
            user.hidden = true
          }
        }
      },
      inputText (event) {
        console.log(event)
        const cursorPosition = this.getInputCursorPosition()
        const supprCharacter = event.inputType === 'deleteContentBackward' || event.inputType === 'deleteContentForward' || event.inputType === 'deletedByCut'

        if (event.data === '@' && this.input_text.charAt(cursorPosition - 2) === ' ') {
          this.in_ref_mode = true
          return
        } else if (event.data === ' ' && (this.in_ref_mode || this.in_edit_mode)) {
          this.in_ref_mode = false
          this.in_edit_mode = false
        } else if (this.input_text.indexOf('@') > -1 && !this.in_ref_mode) {
          for (let i = cursorPosition; i > 0; i--) {
            if (this.input_text.charAt(i) === "@" && this.input_text.charAt(i - 1) === " ") {
              this.in_edit_mode = true
              this.current_search = this.input_text.substring(i + 1, (this.input_text.indexOf(" ", i) > -1 ? this.input_text.indexOf(" ", i) : this.input_text.length) + 1)
              break
            } else if (this.input_text.charAt(i) === " ") {
              this.in_ref_mode = false
              this.in_edit_mode = false
              break
            }
          }
        } else {
          this.in_ref_mode = false
          this.in_edit_mode = false
        }

        if (this.in_ref_mode || this.in_edit_mode) {
          if (this.in_ref_mode && event.data) {
            this.current_search += event.data
          }

          if (supprCharacter) {
            this.current_search = this.current_search.slice(0, -1)
          }

          this.searchList(this.current_search)
        }
      },
      addUserToReference (user) {
        // const cursorPosition = this.getInputCursorPosition()
        const stringToReplace = this.input_text.substring(0, this.input_text.lastIndexOf('@') + 1)
        const stringReady = stringToReplace + user.nick_name + ' '
        console.log('READY', stringToReplace)
        this.input_text = stringReady

        this.$refs.text_element.focus()

        this.references.push({
          id: this.references.length + 1,
          user_ref: user.nick_name,
          user_fullname: user.first_name + user.last_name
        })

        this.in_ref_mode = false
        this.in_edit_mode = false
      }
    },
    watch: {
      current_search () {
        console.log(this.current_search)
      },
      in_ref_mode () {
        if (this.in_ref_mode) {
          this.show_list = true
        } else {
          this.toggleSearchOff()
        }
      },
      in_edit_mode () {
        if (this.in_edit_mode) {
          this.show_list = true
        } else {
          this.toggleSearchOff()
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .user_reference_input_component {
    .users_list {
      margin: 0;
      padding: 0;
      /*position: absolute;*/
      /*top: 0;*/

      .single_user {
        list-style-type: none;
      }
    }
  }
</style>
