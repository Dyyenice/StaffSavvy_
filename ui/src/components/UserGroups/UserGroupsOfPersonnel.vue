<template>
  <div class="container">

    <div v-if="message">  <label class="labelheader">{{ message }}</label></div>
    <div v-if="!message"><label class="labelheader">MY TEAMS</label></div>
    <table class="table">
      <thead>
      <tr>
        <th>Name</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="usergroup in usergroups" :key="usergroup.id">
        <td>{{ usergroup.name }}</td>
        <td>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
import CompanyService from "../../services/company.service";

export default {
  name: "UserGroup",
  components: {

  },
  data() {
    return {
      usergroups: [],
      isEditmode:false,
      message: "",
    };
  },

  computed: {

    currentUser() {
      return this.$store.state.auth.user;
    }

  },
  mounted()
  {
    CompanyService.getUserGroupsOfPersonnel(this.currentUser).then(
        (response) => {
          this.usergroups = response.data;

        },
        (error) => {
          this.message = (error.response &&
                  error.response.data &&
                  error.response.data.message) ||
              error.message ||
              error.toString();
        }
    );

  },
  methods: {
    teamDetails(task){
      localStorage.setItem("selectedTeam", JSON.stringify(task));
      console.log(JSON.parse(localStorage.getItem("selectedTeam")));
      this.$router.push("/UserGroupDetails");
    }



  }

};
</script>