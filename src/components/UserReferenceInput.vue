<template>
  <div class="user_reference_input_component">
    <label>User Reference</label><br>
    <textarea v-model="input_text" rows="20" cols="80" ref="text_element"></textarea>
    <div v-html="input_text"></div>
    <ul class="users_list" v-show="show_list">
      <li class="single_user" v-for="user of users" v-show="!user.hidden" :key="user.id">
        <a href="#" class="single_user_link" @click="addUserToReference(user.nick_name)">{{`@${user.nick_name} (${user.first_name + " " + user.last_name})`}}</a>
      </li>
    </ul>
  </div>
</template>

<script>
  import users from '../users'

  export default {
    data () {
      return {
        input_text: '',
        show_list: false,
        users: users
      }
    },
    methods: {
      getInputCursorPosition () {
        return this.$refs.text_element.selectionStart
      },
      searchList (stringToSearch) {
        for (const user of users) {
          if (user.first_name.indexOf(stringToSearch) > -1 || user.last_name.indexOf(stringToSearch) > -1 || user.nick_name.indexOf(stringToSearch) > -1) {
            user.hidden = false
          } else {
            user.hidden = true
          }
        }
      },
      addUserToReference (userNickname) {
        // const cursorPosition = this.getInputCursorPosition()
        const stringToReplace = this.input_text.substring(this.input_text.indexOf('@') + 1)
        const stringReady = stringToReplace.replace(/.*/, userNickname)
        this.input_text = this.input_text.replace(/(@).*/, '$1' + stringReady + ' ')
        this.$refs.text_element.focus()
      }
    },
    watch: {
      input_text () {
        const atIndex = this.input_text.indexOf('@')
        if (atIndex > -1) {
          this.show_list = true
          this.searchList(this.input_text.substring(atIndex + 1))
        } else {
          this.show_list = false
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .user_reference_input_component {
    .users_list {
      position: absolute;
      top: 0;
    }
  }
</style>
