<template>
  <div class="container">


    <div ><label class="labelheader">COMPANY ROLE GROUPS</label></div>
    <table class="table">
      <thead>
      <tr>
        <th>Name</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="rolegroup in rolegroups" :key="rolegroup.id">
        <td>{{ rolegroup.name }}</td>
        <td>
          <button class="btn btn-primary form-group col-md-2" :disabled="loading" type="submit"  @click="rolegroupDetails(rolegroup)"  >
            <span v-show="loading" class="spinner-border spinner-border-sm"></span>
            <span>Details</span>
          </button>
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
      rolegroups: [],
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
    CompanyService.getRolegroups(this.currentUser).then(
        (response) => {
          this.rolegroups = response.data;

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
    rolegroupDetails(rolegroup){
      localStorage.setItem("selectedRolegroup", JSON.stringify(rolegroup));
      console.log(JSON.parse(localStorage.getItem("selectedRolegroup")));
      this.$router.push("/RolegroupDetails");
    }



  }

};
</script>