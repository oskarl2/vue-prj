<template>
    <div class="modal-wrapper__form-wrapper">
      <form id="form" v-on:submit.prevent="saveUser">
        <h2 class="modal-wrapper__form-header">Добавление пользователя</h2>
        <span class="modal-wrapper__form-close" @click='close'></span>
        <div class="modal-wrapper__form-input_wrapper">
          <div>
            <label>Имя</label><input class="modal-wrapper__form-input_item" type="text" v-model="newUser.name" placeholder="Имя">
            <div class="errors" v-show="!validation.name">Введите Имя</div>
          </div>
          <div>
            <label>Телефон</label><input class="modal-wrapper__form-input_item" type="tel" v-model="newUser.phone" placeholder="Телефон">
            <div class="errors" v-show="!validation.phone">Введите корректный номер</div>
          </div>
          <div>
            <label>
              <select v-model="newUser.worker" class="modal-wrapper__form-select">
                <option value="def">Choose your destiny</option>
                <option v-for="user in users">{{user.name}}</option>
              </select>
            </label>
          </div>
        </div>
        <v-button type="submit" class="modal-wrapper__form-button">Сохранить</v-button>
      </form>
    </div>
</template>

<script>
import Button from './Button.vue';

export default {

  name: 'modal',

  components: {
    'v-button': Button,
  },

  props: [
    'users',
    'showHideModal',
    'addUser',
    'addAdditionalUser'
  ],

  data() {
    return {
      selected: 'def',
      newUser: {
        name: '',
        phone: '',
        worker: '',
        showModal: Boolean
        },
      }
  },

  computed: {
    validation() {
      const phoneRE = /^(\s*)?(\+)?([- _():=+]?\d[- _():=+]?){10,14}(\s*)?$/;
      return {
        name: !!this.newUser.name.trim(),
        phone: phoneRE.test(this.newUser.phone)
      }
    },

    isValid() {
      let validation = this.validation;
      return Object.keys(validation).every(function (key) {
        return validation[key];
      })
    }
  },

  methods: {
    close() {
      this.showHideModal({
        showModal: false
      })
    },

    addNewUser(newUser) {
      this.addUser(newUser)
    },

    addNewAdditionalUser(newUser) {
      this.addAdditionalUser(newUser)
    },

    saveUser() {
      let newUser = {
        name: '',
        phone: '',
        id: '',
        isShow: true
      };

      if(this.isValid) {
        newUser = {
          name: this.newUser.name,
          phone: this.newUser.phone,
          worker: this.newUser.worker,
          id: Math.random().toString(36).substr(2, 6),
          isShow: true
        };

        if(this.newUser.worker) {
          this.addNewAdditionalUser(newUser);
        } else {
          this.addNewUser(newUser);
        }

      }
    },
  }
}
</script>

<style scoped>
  .modal-wrapper__form-wrapper {
    margin-left: 50px;
    padding: 10px 20px 25px;
    background: rgba(255,246,254,0.95);
    border: 1px solid darkgray;
    width: 400px;
    position: relative;
    height: max-content;
    border-radius: 5px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  }

  .modal-wrapper__form-label {
    padding-right: 10px;
  }

  .modal-wrapper__form-header {
    font-size: 16px;
    margin-bottom: 30px;
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

  .modal-wrapper__form-button:disabled {
    background: rgb(221, 221, 221);
    cursor: not-allowed;
  }

  .modal-wrapper__form-button:active {
    background: rgba(236,226,237,0.95);
  }

  .modal-wrapper__form-close  {
    position: absolute;
    width: 10px;
    height: 10px;
    top: 10px;
    right: 10px;
  }

  .modal-wrapper__form-close:hover  {
    cursor: pointer;
  }

  .modal-wrapper__form-close::after {
    content: '\00D7';
    transform: scale(1.3);
    display: block;
  }

  .modal-wrapper__form-input_wrapper {
    display: flex;
    flex-direction: column;
  }

  .modal-wrapper__form-input_item {
    margin-left: 10px;
    margin-bottom: 13px;
    width: 55%;
    border: 1px solid darkgray;
  }

  .modal-wrapper__form-select {
    margin-bottom: 20px;
    width: 55%;
    border: 1px solid darkgray;
  }

  select, option {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  }

  .errors {
    height: 16px;
    margin-bottom: 15px;
    color: rgba(60,134,237,0.95);
  }
</style>