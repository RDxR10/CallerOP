<template>
  Enter a phonenumber:
  <input type="text" v-model="phonenumber" name="phonenumber" />
  <br>
  <br>
  <button v-on:click="getOperator()" :disabled="isDisabled" class="btn">
    <span>
      Check Operator
    </span>
  </button>
  <button v-on:click="clearData()" class="btn">
    <span>
      Clear
    </span>
  </button>
  <span class="loader" v-if="isLoading">
  </span>
  <br>
  <br>
  <span>
    {{this.data}}
  </span>
  <br>
  <br>
  <br>
  <br>
  <span>
    Recent Searches:
    <ul>
      <li v-for="(number, index) in recentNumbers.slice().reverse()" :key="index">
      {{ number }}
      </li>
    </ul>
  </span>
</template>
<script>
  import axios from 'axios'

  export
default {
    name:
    'App',
    data() {
      return {
        phonenumber: '',
        validPhoneNumber: false,
        isLoading: false,
        recentNumbers: [],
      };
    },
    computed: {
      isDisabled() {
        return ! this.validPhoneNumber;
      }
    },

    methods: {
      async getOperator() {
        try {
          this.isLoading = true;
          const url = `http://localhost:8081/operator?phonenumber=${this.phonenumber}`;
          let result = await axios.get(url);
          this.data = JSON.stringify(result.data.operator).replace(/"/g, '');
          this.recentNumbers.push(this.phonenumber);
        } catch(error) {
          console.error(error);
          alert(error.response.data.error);
        } finally {
          this.isLoading = false;
        }

      },
      clearData() {
        this.phonenumber = '';
        this.data = '';
      }
    },

    watch: {
      phonenumber: function() {
        const regex = /^\d{10}$/;
        this.validPhoneNumber = regex.test(this.phonenumber);
      }
    },

  };
</script>
<style>
  @import url('https://fonts.googleapis.com/css?family=Avenir'); body {
  font-family: Avenir; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing:
  grayscale; text-align: center; color: #f4f6f8; margin-top: 60px; background:
  #2c3e50; } .btn { width: 120px; height: 30px; cursor: pointer; background:
  #2c3e50; border: 2px solid #8a9bab; outline: none; transition: 1s ease-in-out;
  } .btn-container { position: relative; } .btn:hover { transition: 0.25s
  ease-in-out; background: #51bea3; } .btn span { color: white; font-size:
  15px; font-weight: 100; } .btn:disabled { opacity: 0.5; cursor: not-allowed;
  background: #f7856f; } .loader { width: 28px; height: 28px; border: 5px
  dotted #FFF; border-radius: 50%; display: inline-block; position: absolute;
  top: 13.75%; transform: translateY(-50%); margin-left: 0.55cm; box-sizing:
  border-box; animation: rotation 2s linear infinite; } @keyframes rotation
  { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }ul { list-style-type: none; }
</style>

