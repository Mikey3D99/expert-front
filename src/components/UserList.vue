<template>
    <div>
    <input type="text" v-model="searchText" placeholder="Search by email" />
    <button @click="searchUser" class="search-button">Search</button>
    <button @click="getUsers" class="modern-button">Reload Users</button>
    <button @click="addUser" class="add-button">Add User</button>
    <table class="centered">
        <tr class="centered">
          <th>ID</th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Email</th>
          <th>Age</th>
        </tr>
        <tr v-for="user in filteredUsers" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.firstName }}</td>
          <td>{{ user.lastName }}</td>
          <td>{{ user.email}}</td>
          <td>{{ user.age}}</td>
          <td>
            <button @click="deleteUser(user.id)" class="delete-button">Delete</button>
          </td>
        </tr>
      </table>
    </div>
  </template>

<script>
import axios from 'axios'
    export default {
        name: 'UserList',
        data(){
            return{
                users: [],
                dataFetched: false,
                searchText: '',
                filteredUsers: []
            }
        },
        created() {
            if (!this.dataFetched) {
                this.getUsers()
                this.dataFetched = true
             }
        },
        methods:{
            getUsers(){
                axios.get('http://localhost:8083/api/clients')
                .then((response) => {
                    console.log(response.data)
                    this.users = response.data
                    this.filteredUsers = response.data

                })
                .catch(error=>{
                    console.log(error)
                })
            },
            deleteUser(id) {
                if (confirm('Are you sure you want to delete this user?')) {
                    axios.delete(`http://localhost:8083/api/clients/${id}`)
                    .then(response => {
                        // Reload the users list
                        console.log(response);
                        this.getUsers();
                    })
                    .catch(error => {
                        console.log(error);
                    });
                }
            },
            searchUser(){
                this.filteredUsers = this.filteredUsers.filter(user => user.email.includes(this.searchText))
                console.log(this.filteredUsers)
            },
            addUser(){

            }
        }
    }
</script>

<style scoped>
    .modern-button {
  display: inline-block;
  font-size: 16px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-decoration: none;
  padding: 15px 30px;
  border: none;
  border-radius: 4px;
  background-color: #0b54b3;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.1s ease-in-out;
  margin: 20px;
}


.modern-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.modern-button:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.delete-button {
  display: inline-block;
  font-size: 16px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-decoration: none;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: red;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.1s ease-in-out;
  margin: 20px;
}

.delete-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.delete-button:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

table {
  border-collapse: collapse;
  border: 1px solid #ddd;
  margin: 10px;
}

td, th {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

th {
  text-align: left;
}

table.centered {
  margin: auto;
  width: 100%;
}

input[type="text"] {
  padding: 16px;
  border: 1px solid #ccc;
  background-color: #fff;
  transition: all 0.2s ease-in-out;
  border-radius: 4px;
}

input[type="text"]:focus {
  border: 1px solid #4caf50;
  box-shadow: 0 0 10px #4caf50;
}

.search-button {
  display: inline-block;
  font-size: 16px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-decoration: none;
  padding: 15px 30px;
  border: none;
  border-radius: 4px;
  background-color: #0b54b3;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.1s ease-in-out;
  margin: 20px;
}

.search-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.search-button:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.add-button {
  display: inline-block;
  font-size: 16px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-decoration: none;
  padding: 15px 30px;
  border: none;
  border-radius: 4px;
  background-color: #0b54b3;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.1s ease-in-out;
  margin: 20px;
}

.add-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.add-button:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}
</style>