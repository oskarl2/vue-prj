<template>
  <div class="main_page">
    <div class="main_page_inner">
      <v-button v-on:click.native="showHideModal(showModal)" class="main_page_button modal-wrapper__form-button">
        {{ button.text }}
      </v-button>
      <v-table :users="users"
               :sortByPhone="sortByPhone"
               :sortByName="sortByName"
               :deleteUsr="deleteUsr">

      </v-table>
    </div>
    <v-modal v-if="showModal" v-bind:users="users"
             :showHideModal="showHideModal"
             :addUser="addUser"
             :addAdditionalUser="addAdditionalUser">
    </v-modal>
  </div>
</template>

<script>
  import Modal from './Modal.vue';
  import Table from './Table.vue';
  import Button from './Button.vue';

  export default {
    components: {
      'v-table': Table,
      'v-modal': Modal,
      'v-button': Button
    },

    name: 'Main',

    props: {
      msg: String,
    },

    data() {
      return {
        button: {
          text: 'Показать форму'
        },
        newUser: {
          name: '',
          phone: '',
          id: ''
        },
        showModal: false,
        users: [],
        isSortedByName: false
      }
    },

    mounted() {
      if (localStorage.users) {
        this.users = JSON.parse(localStorage.users);
      } else {
        localStorage.users = JSON.stringify(this.users)
      }
    },

    methods: {
      addUser(newUser) {
        this.users.push(newUser);
        this.users.forEach(function (user, idx) {
          user.id = idx;
          user['isShow'] = true;
        });

        localStorage.users = JSON.stringify(this.users);
      },

      addAdditionalUser(newUser) {
        let self = this;
        this.users.forEach(function (user) {
          if (user.name === newUser.worker) {
            if (user.additionalUsers) {
              user.additionalUsers.push(newUser);
            } else {
              self.$set(user, 'additionalUsers', [newUser]);
            }
          }
        });

        localStorage.users = JSON.stringify(this.users);
      },

      deleteUsr(user) {
        this.users.splice(this.users.indexOf(user), 1);
        localStorage.users = JSON.stringify(this.users);
      },

      showHideModal(value) {
        this.showModal = !value;
        this.button.text = value ? 'Показать форму' : 'Скрыть форму';
      },

      sortByPhone() {
        if (!this.isSorted) {
          this.users.sort(function (a, b) {
            let nameA = a.phone.toLowerCase(), nameB = b.phone.toLowerCase();
            if (nameA < nameB)
              return -1;
            if (nameA > nameB)
              return 1;
            return 0
          });
          this.isSorted = true;
        } else {
          this.users.sort(function (a, b) {
            let nameA = a.phone.toLowerCase(), nameB = b.phone.toLowerCase()
            if (nameA > nameB)
              return -1;
            if (nameA < nameB)
              return 1;
            return 0
          });
          this.isSorted = false;
        }
      },

      sortByName() {
        if (!this.isSorted) {
          this.users.sort(function (a, b) {
            let nameA = a.name.toLowerCase(), nameB = b.name.toLowerCase();
            if (nameA < nameB)
              return -1;
            if (nameA > nameB)
              return 1;
            return 0
          });
          this.isSorted = true;
        } else {
          this.users.sort(function (a, b) {
            let nameA = a.name.toLowerCase(), nameB = b.name.toLowerCase();
            if (nameA > nameB)
              return -1;
            if (nameA < nameB)
              return 1;
            return 0
          });

          this.isSorted = false;
        }
      },
    }
  }
</script>

<style scoped>
  .main_page_inner {
    position: relative;
  }

  .main_page_button {
    position: absolute;
    top: -40px;
    left: 0;
  }

  .main_page {
    display: flex;
    justify-content: center;
    padding: 20px 60px 40px;
    margin-top: 50px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  }

  .modal-wrapper__form-button {
    cursor: pointer;
    border: 1px solid darkgray;
    border-radius: 10px;
    padding: 5px 7px;
    width: 120px;
    text-align: left;
    outline: none;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  }

  button {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  }
</style>