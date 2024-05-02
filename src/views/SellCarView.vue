<template>
  <v-container>
    <v-row class="text-center">
        <v-col cols="12">
            <h1>Advertise your car!</h1>
        </v-col>
    </v-row>
    <v-row class="mt-12 mb-4">
      <v-col cols="12" sm="8" md="12">
        <v-card class="pa-6" elevation="6">
          <form>
            <v-text-field
              v-model="model"
              :error-messages="modelErrors"
              :counter="30"
              label="Model"
              required
              @input="$v.model.$touch()"
              @blur="$v.model.$touch()"
            ></v-text-field>
            <v-text-field
              v-model="description"
              :error-messages="descriptionErrors"
              :counter="30"
              label="Description"
              required
              @input="$v.description.$touch()"
              @blur="$v.description.$touch()"
            ></v-text-field>
            <v-row align="center">
              <v-col cols="5">
                <v-text-field
                  v-model="year"
                  :error-messages="yearErrors"
                  :counter="4"
                  label="Year"
                  required
                  @input="$v.year.$touch()"
                  @blur="$v.year.$touch()"
                  @keypress="onlyNumbers"
                ></v-text-field>
              </v-col>
              <v-col cols="2" class="text-center">
                <div>/</div>
              </v-col>
              <v-col cols="5">
                <v-text-field
                  v-model="year2"
                  :error-messages="year2Errors"
                  :counter="4"
                  label="Year"
                  required
                  @input="$v.year2.$touch()"
                  @blur="$v.year2.$touch()"
                  @keypress="onlyNumbers"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-text-field
              v-model="km"
              :error-messages="kmErrors"
              :counter="10"
              label="KM"
              required
              @input="$v.km.$touch()"
              @blur="$v.km.$touch()"
              @keypress="onlyNumbers"
            ></v-text-field>
            <v-text-field
              v-model="value"
              :error-messages="valueErrors"
              :counter="30"
              label="Value"
              required
              @input="$v.value.$touch()"
              @blur="$v.value.$touch()"
              @keypress="onlyNumbers"
            ></v-text-field>
            <v-text-field
              v-model="state"
              :error-messages="stateErrors"
              :counter="30"
              label="State"
              required
              @input="$v.state.$touch()"
              @blur="$v.state.$touch()"
            ></v-text-field>
            <v-text-field
              v-model="city"
              :error-messages="cityErrors"
              :counter="30"
              label="City"
              required
              @input="$v.city.$touch()"
              @blur="$v.city.$touch()"
            ></v-text-field>
            <v-card-title>Add image URLs</v-card-title>
            <v-text-field
              v-model="url"
              :error-messages="urlErrors"
              label="Url 1"
              required
              @input="$v.url.$touch()"
              @blur="$v.url.$touch()"
            ></v-text-field>
            <div class="mb-5">
              <div
                v-for="(textField, index) in textFields"
                :key="index"
                class="d-flex align-center"
              >
                <v-text-field
                  v-model="textField.value"
                  :label="textField.label + (index + 2)"
                  class="mr-2"
                ></v-text-field>
                <v-btn
                  @click="removeTextField(index)"
                  rounded
                  style="font-size: 16px"
                  color="error"
                  >X</v-btn
                >
              </div>
              <v-btn @click="addTextField" rounded color="success"
                ><v-icon>mdi-plus</v-icon></v-btn
              >
            </div>
            <v-checkbox
                v-model="checkbox"
                label="Featured offer?"
                @change="$v.checkbox.$touch()"
                @blur="$v.checkbox.$touch()"
            ></v-checkbox>
            <div class="text-center">
              <v-btn class="mr-4" @click="submit" color="success">
                SALVAR
              </v-btn>
              <v-btn @click="clear" color="error"> LIMPAR </v-btn>
            </div>
          </form>
        </v-card>
      </v-col>
      <v-dialog v-model="dialog" hide-overlay persistent width="300">
        <v-card>
          <v-card-text>
            Car create successfully!
            <v-icon color="green darken-2" class="ml-2 mr-2"
              >mdi-check-circle</v-icon
            >
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-row>
  </v-container>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, maxLength } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],

  validations: {
    model: { required, maxLength: maxLength(30) },
    description: { required, maxLength: maxLength(10) },
    year: { required, maxLength: maxLength(4) },
    year2: { required, maxLength: maxLength(4) },
    km: { required, maxLength: maxLength(10) },
    value: { required, maxLength: maxLength(30) },
    state: { required, maxLength: maxLength(30) },
    city: { required, maxLength: maxLength(30) },
    url: { required },
    checkbox: {
        checked (val) {
          return val
        },
    }
  },

  data: () => ({
    model: "",
    description: "",
    year: "",
    year2: "",
    km: "",
    value: "",
    state: "",
    city: "",
    url: "",
    textFields: [{ value: "", errors: [], label: "Url " }],
    checkbox: false,
    urls: [],
    dialog: false,
    sucess: false,
  }),
  watch: {
    dialog(val) {
      if (!val) return;

      setTimeout(() => (this.dialog = false), 4000);
    },
  },

  computed: {
    modelErrors() {
      const errors = [];
      if (!this.$v.model.$dirty) return errors;
      !this.$v.model.maxLength &&
        errors.push("Model must be at most 30 characters long");
      !this.$v.model.required && errors.push("Model is required.");
      return errors;
    },
    descriptionErrors() {
      const errors = [];
      if (!this.$v.description.$dirty) return errors;
      !this.$v.description.maxLength &&
        errors.push("Description must be at most 30 characters long");
      !this.$v.description.required && errors.push("Description is required.");
      return errors;
    },
    yearErrors() {
      const errors = [];
      if (!this.$v.year.$dirty) return errors;
      !this.$v.year.maxLength &&
        errors.push("Year must be at most 4 characters long");
      !this.$v.year.required && errors.push("Year is required.");
      return errors;
    },
    year2Errors() {
      const errors = [];
      if (!this.$v.year2.$dirty) return errors;
      !this.$v.year2.maxLength &&
        errors.push("Year must be at most 4 characters long");
      !this.$v.year2.required && errors.push("Year is required.");
      return errors;
    },
    kmErrors() {
      const errors = [];
      if (!this.$v.km.$dirty) return errors;
      !this.$v.km.maxLength &&
        errors.push("Km must be at most 10 characters long");
      !this.$v.km.required && errors.push("Km is required.");
      return errors;
    },
    valueErrors() {
      const errors = [];
      if (!this.$v.value.$dirty) return errors;
      !this.$v.value.maxLength &&
        errors.push("Value must be at most 30 characters long");
      !this.$v.value.required && errors.push("Value is required.");
      return errors;
    },
    stateErrors() {
      const errors = [];
      if (!this.$v.state.$dirty) return errors;
      !this.$v.state.maxLength &&
        errors.push("State must be at most 30 characters long");
      !this.$v.state.required && errors.push("State is required.");
      return errors;
    },
    cityErrors() {
      const errors = [];
      if (!this.$v.city.$dirty) return errors;
      !this.$v.city.maxLength &&
        errors.push("City must be at most 30 characters long");
      !this.$v.city.required && errors.push("City is required.");
      return errors;
    },
    urlErrors() {
      const errors = [];
      if (!this.$v.url.$dirty) return errors;
      !this.$v.url.required && errors.push("Url is required.");
      return errors;
    },
  },

  methods: {
    async submit() {
      this.$v.$touch();

      this.dialog = true;

      if (
        this.model === "" ||
        this.description === "" ||
        this.year === "" ||
        this.year2 === "" ||
        this.km === "" ||
        this.value === "" ||
        this.state === "" ||
        this.city === "" ||
        this.url === ""
      ) {
        this.dialog = false;
        return;
      }

      this.urls.push({ src: this.url });

      for (let i = 0; i < this.textFields.length; i++) {
        this.urls.push({ src: this.textFields[i].value });
      }

      const data = {
        model: this.model,
        description: this.description,
        year: this.year,
        year2: this.year2,
        km: this.km,
        value: this.value,
        state: this.state,
        city: this.city,
        url: Array.from(this.urls),
        offer: this.checkbox,
        favorite: false
      };

      const dataJson = JSON.stringify(data);

      let apiUrl = "";

      if (
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
          navigator.userAgent
        )
      ) {
        apiUrl = "http://10.4.0.74:3000/cars";
      } else {
        apiUrl = "http://localhost:3000/cars";
      }

      const req = await fetch(apiUrl, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      if (res) {
        setTimeout(() => {
          this.dialog = false;
          this.clear();
        }, 2000);
      }
    },
    clear() {
      this.$v.$reset();
      this.model = "";
      this.description = "";
      this.year = "";
      this.year2 = "";
      this.km = "";
      this.value = "";
      this.state = "";
      this.city = "";
      this.url = "";
      this.textFields.forEach((field) => {
        field.value = "";
        field.errors = [];
      });
      this.urls = [];
      this.checkbox = false
    },
    addTextField() {
      this.textFields.push({
        value: "",
        errors: [],
        label: "Url ",
      });
    },
    removeTextField(index) {
      this.textFields.splice(index, 1);
    },
    onlyNumbers(event) {
      const keyCode = event.keyCode;
      if (keyCode < 48 || keyCode > 57) {
        event.preventDefault();
      }
    },
  },
};
</script>