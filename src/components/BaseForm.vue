<template>
  <div class="base-form shadow">
      <div class="row one">
        <span>Filter prefix:</span>
        <input type="text" v-model="search">
      </div>
      <div class="row two">
          <select 
            v-model="selected" 
            onmousedown="if(this.options.length>21){this.size=21}"  
            onchange='this.size=0;' 
            onblur="this.size=0;"
          >
            <option value="none"></option>
            <option 
              v-for="(name, index) in filteredNames" 
              :key="index"
              :value="name"
            >
              {{ name.first_name }} {{ name.last_name }}
            </option>
          </select>
          <div class="name-fields">
              <div class="input-container">
                <span>Name:</span>
                <input v-model="firstName" type="text">
              </div>
              <div class="input-container">
                <span>Surname:</span>
                <input v-model="lastName" type="text">
              </div>
          </div>
      </div>
      <Actions 
        :noneIsSelected="noneIsSelected"
        :disabledCreate="disabledCreate"
        :disabledUpdate="disabledUpdate"
        @create="create"
        @update="update"
        @remove="remove"
      />
  </div>
</template>

<script>
  import axios from "axios";
  import Actions from "./Actions.vue"

  export default {
      name: 'Idiomatic',

      mounted() {
        this.initNames();
      },

      components: {
        Actions
      },

      data() {
        return {
          names: [],
          search: "",
          selected: "none",
          firstName: "",
          lastName: "",
          url: "http://localhost:3000/names"
        }
      },

      computed: {
        filteredNames: function() {
          if (this.search) {
            return this.names.filter( name => {
            const a = name.last_name.toUpperCase();
            const b = this.search.toUpperCase();
                    
            return a.match(b);
            })
          }
        
          return this.names;
          },

        noneIsSelected() {
          return this.selected === "none" || !this.names.length;
        },
        
        nameIsComplete() {
          return this.firstName && this.lastName;
        },

        disabledCreate() {
          return !this.noneIsSelected || !this.nameIsComplete;
        },

        disabledUpdate() {
          return this.noneIsSelected || !this.nameIsComplete;
        },

        selectedIndex() {
          return this.names.indexOf(this.selected);
        },

        userUrl() {
          return `${this.url}/${this.names[this.selectedIndex].id}`;
        }
      },

      methods: {
        initNames() {               
          axios.get(this.url).then( response => {
            this.names = response.data;
          })
        },

        create() {
          const id = Math.random();

          axios.post(this.url, {
            id: id,
            first_name: this.firstName,
            last_name: this.lastName
          }).then(() => {
            this.initNames();
            this.resetFields();
          }).catch(error => {
            console.log(error);
          });
        },

        update() {
          axios.put(this.userUrl, {
            first_name: this.firstName,
            last_name: this.lastName
          }).then(() => {
            this.initNames();
            this.resetFields();
          }).catch(error => {
            console.log(error);
          });
        },

        remove() {
          axios.delete(this.userUrl).then(() => {
            this.initNames();
            this.resetFields();
          }).catch(error => {
            console.log(error);
          });
        },

        resetFields() {
          this.firstName = "";
          this.lastName = "";
          this.selected = "none";
        }
      }
    }
</script>

<style>

  .base-form {
    max-width: 450px;
    display: block;
    margin: 20px auto;
    background: white;
    border-radius: 7px;
    padding: 10px;
  }

  .base-form span {
    color: black;
    font-size: 16px;
    font-weight: 500;
    padding: 10px;
  }

  .row {
    display: flex;
    justify-content: space-between;
    padding: 10px;
  }

  input, select {
    border: 1px solid #c7c7c7;
    border-radius: 4px;
    padding: 10px;
  }

  select {
    max-height: 200px;
    width: -webkit-fill-available;
    height: fit-content;
  }

  .row.one span {
    white-space: nowrap;
    padding-left: 0;
  }

  .row.one input {
    width: -webkit-fill-available;
  }

  .name-fields {
    display: block;
  }

  .name-fields  .input-container {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 10px;
  }

  .shadow {
    -webkit-box-shadow: 1px 0px 4px 1px rgba(0,0,0,0.21); 
    box-shadow: 1px 0px 4px 1px rgba(0,0,0,0.21);
  }
  
</style>
