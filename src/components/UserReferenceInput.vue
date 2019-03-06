<template>
  <div class="user_reference_input_component">
    <label>User Reference</label><br>
    <textarea v-model="input_text" rows="20" cols="80"></textarea>
    <ul class="users_list" v-show="show_list">
      <li class="single_user" v-for="user of users" :key="user.id">{{`@${user.nick_name} (${user.first_name + " " + user.last_name})`}}</li>
    </ul>
  </div>
</template>

<script>
  import users from '../users'

  export default {
    data () {
      return {
        input_text: '',
        show_list: false
      }
    },
    computed: {
      users () {
        return users
      }
    },
    watch: {
      input_text () {
        if (this.input_text.indexOf('@') > -1) {
          console.log(this.input_text)
          this.show_list = true
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
