<template>
  <div id="employee-from">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
      ref="first"
        @focus="clearStatus"
        :class="{ 'has-error': submitting && invalidName }"
        @keypress="clearStatus"
        v-model="employee.name"
        type="text"
      />
      <label>Employee Email</label>
      <input
        :class="{ 'has-error': submitting && invalidName }"
        @focus="clearStatus"
        v-model="employee.email"
        type="text"
      />
      <p v-if="error && submitting" class="error-message">
          ! Hey dfkjds
      </p>
      <p v-if="success" class="success-message"> SuccessFull  </p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: "",
      },
    };
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },

    invalidEmial() {
      return this.employee.email === "";
    },
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmial) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.$refs.first.focus()
      this.employee = {
        name: "",
        email: "",
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
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