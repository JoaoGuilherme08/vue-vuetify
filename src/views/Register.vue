<template>
  <v-card
    width="800"
    class="mx-auto mt-5"
    color="indigo lighten-4"
    elevation="20"
  >
    <v-card-title><h1 class="display-1">Register</h1></v-card-title>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-container>
        <v-text-field
          v-model="name"
          :counter="40"
          :rules="nameRules"
          label="Name"
          required
        ></v-text-field>

        <v-text-field
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
        ></v-text-field>

        <v-row>
          <v-col cols="5">
            <v-text-field
              label="CEP"
              @blur="getCEP"
              v-model="cep"
            ></v-text-field>
          </v-col>
          <v-col cols="5">
            <v-text-field label="Cidade" v-model="cidade"></v-text-field>
          </v-col>
          <v-col cols="2">
            <v-text-field label="Estado" v-model="estado"></v-text-field>
          </v-col>
        </v-row>

        <v-text-field
          v-model="Password"
          :rules="passwordRules"
          label="Password"
          required
        ></v-text-field>

        <v-text-field
          v-model="confirmPassword"
          :rules="confirmPasswordRules"
          label="Confirm password"
          v-on:blur="confirm()"
          required
        ></v-text-field>
        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="validate"
        >
          Validate
        </v-btn>

        <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>

        <v-btn color="warning" @click="resetValidation">
          Reset Validation
        </v-btn>
      </v-container>
    </v-form>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 40) || "Name must be less than 40 characters",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    Password: "",
    passwordRules: [(v) => !!v || "Password is required"],

    confirmPassword: "",
    confirmPasswordRules: [(v) => !!v || "Password is required"],

    cep: "",
    cidade: "",
    estado: "",
  }),

  methods: {
    validate() {
      this.$refs.form.validate();
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    async getCEP() {
      if (this.cep.length === 8) {
        const response = await fetch(
          "https://viacep.com.br/ws/" + this.cep + "/json/"
        );
        const data = await response.json();

        this.cidade = data.localidade;
        this.estado = data.uf;
      } else {
        console.log("CEP errado.");
      }
    },
  },
};
</script>
