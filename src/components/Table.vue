<template>
  <div class="main_page_table_wrapper">
    <table>
      <thead>
      <tr>
        <th @click="sortName">Имя</th>
        <th @click="sortPhone">Телефон</th>
      </tr>
      </thead>
      <tbody v-if="users.length">
      <template v-for="user in users" >
        <tr :key="user.id">
          <td v-bind:class="{'toggler': !!user.additionalUsers }" @click="toggler(user)">{{user.name}}</td>
          <td>{{user.phone}}<v-button class="delete" v-on:click.native="deleteUser(user)">Удалить</v-button></td>
        </tr>
        <template v-if="user.additionalUsers" >
          <tr v-for="(additionalUser, index) in user.additionalUsers" :key="user.additionalUsers.id" v-show="user.isShow" class="add_user">
            <td>{{additionalUser.name}}</td>
            <td>{{additionalUser.phone}}</td>
          </tr>
        </template>
      </template>
      </tbody>
      <tbody v-else>
      <tr>
        <td colSpan=2>Заполните пожалуйста Данные</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Button from './Button.vue';

  export default {
    name: 'Table',

    components: {
      'v-button': Button,
    },

    data() {
      return {
        isShow: true,
      }
    },

    props: [
      'users',
      'sortByName',
      'sortByPhone',
      'deleteUsr'
    ],

    methods: {
      toggler(user) {
        if(user.additionalUsers) {
          user.isShow = !user.isShow
        }
      },

      deleteUser(user) {
        this.deleteUsr(user);
      },

      sortName() {
        this.sortByName();
      },

      sortPhone() {
        this.sortByPhone();
      }
    }
  }
</script>

<style scoped>
  .add_user {
    background: #d7e8ff;
  }

  table {
    min-width: 400px;
    border-collapse: collapse;
    background: rgba(255,246,254,0.95)!important;
  }

  td,th {
    border: 1px solid #ccc !important;
    padding: 5px 10px 5px 10px !important;
  }

  th {
    cursor: pointer;
    user-select: none;
  }

  .delete {
    font-size: 10px;
    border-radius: 5px;
    border: 1px solid lightgray;
    background: #fff;
    outline: none;
    box-shadow: none;
    -webkit-box-shadow: none;
    position: absolute;
    right: 10px;
  }

  .delete:hover  {
    cursor: pointer;
  }

  .toggler {
    padding-left: 33px !important;
    position: relative;
  }

  .toggler::before {
    content: url("../assets/icon_fall.png");
    position: absolute;
    left: 8px;
    transform: scale(0.6);
  }
</style>