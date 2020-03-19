<template>
  <div class="users-edit">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Edit User Information</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Email:</label>
          <input type="text" class="form-control" v-model="user.email" />
        </div>
        <div class="form-group">
          <label>Password:</label>
          <input type="password" class="form-control" v-model="user.password" />
        </div>
        <div class="form-group">
          <label>Confirm Password:</label>
          <input type="password" class="form-control" v-model="user.password_confirmation" />
        </div>
        <div class="form-group">
          <label>First Name:</label>
          <input type="text" class="form-control" v-model="user.first_name" />
        </div>
        <div class="form-group">
          <label>Last Name:</label>
          <input type="text" class="form-control" v-model="user.last_name" />
        </div>

        <div class="form-group">
          <label for="skill_levels">Skill Level:</label>
          <select id="skill_levels" type="text" class="form-control" v-model="user.skill_level">
            <option value="novice">Novice</option>
            <option value="intermediate">Intermediate</option>
            <option value="advanced">Advanced</option>
          </select>
        </div>

        <input type="submit" class="btn btn-primary" value="Update User" />
        <br />
        <br />
        <div>
          <button v-on:click="destroyUser()">Delete Account</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      user: {},
      errors: []
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {
    submit: function() {
      var params = {
        first_name: this.user.first_name,
        last_name: this.user.last_name,
        skill_level: this.user.skill_level,
        email: this.user.email,
        password: this.user.password,
        password_confirmation: this.user.password_confirmation
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
        .then(response => {
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyUser: function() {
      confirm("Are you sure you want to delete your account?");
      axios
        .delete(`/api/users/${this.user.id}`)
        .then(response => {
          console.log("Account deleted.", response.data);
          this.$router.push("/home");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
      delete axios.defaults.headers.common["Authorization"];
      localStorage.removeItem("jwt");
      localStorage.removeItem("user_id");
      this.$router.push("/");
    }
  }
};
</script>
