<template>
  <div id="employee-form">
    <!-- memberikan onsubmit event dengan menambahkan @submit-->
    <!-- .prevent sama artinya dengan event.preventDefault-->
    <form @submit.prevent="handleSubmit">
      <label>Employee Name</label>
      <input
        ref="first"
        v-model="employee.name"
        type="text"
        :class="{ 'has-error': submit && invalidNama }"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Employee Email</label>
      <input
        v-model="employee.email"
        type="text"
        :class="{ 'has-error': submit && invalidEmail }"
        @focus="clearStatus"
        ref="first"
      >
      <p v-if="error & submit" class="error-message">📛 tolong diisi semua !</p>
      <p v-if="success" class="success-message">☑️ Employee berhasil di tambahkan</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submit: false,
      error: false,
      success: false,
      employee: {
        name: " ",
        email: " "
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submit = true;
      this.clearStatus();

      if (this.invalidNama || this.invalidEmail) {
        this.error = true;
        return;
      }
      //emit berfungsi untuk broadcat nama dari event serta data yang di passing ke komponen parent
      //this.$emit('nama-event-yangdi-emmit', 'data-yang-di-passing')
      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: ""
      };
      this.error = false;
      (this.success = true), (this.submit = false);
    },
    clearStatus() {
      (this.success = false), (this.error = false);
    }
  },
  computed: {
    invalidNama() {
      return this.employee.name === "";
    },
    invalidEmail() {
      return this.employee.email === "";
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
