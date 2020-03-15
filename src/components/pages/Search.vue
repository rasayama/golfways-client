<template>
  <div>
    <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <v-text-field
      v-model="name"
      :rules="nameRules"
      label="出発地点"
      required
      ></v-text-field>
      <v-select
        v-model="select"
        :items="items"
        :rules="[v => !!v || 'Item is required']"
        label="ゴルフ場名"
        required
      ></v-select>
    </v-form>

    <div class="dummy-map">
      <div class="dummy-text">
          [ Map Sample ]<br />
        <span v-show="select == null">
          Unselected
        </span>
        <span v-show="select != null">
          Route to {{ select }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters',
      ],
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      select: null,
      items: [
        'Golf Course 1',
        'Golf Course 2',
        'Golf Course 3',
        'Golf Course 4',
      ],
    }),

    methods: {
      validate () {
        if (this.$refs.form.validate()) {
          this.snackbar = true
        }
      },
      reset () {
        this.$refs.form.reset()
      }
    },
  }
</script>

<style scoped>
form {
  width: 70%;
  max-width: 600px;
  margin: 30px auto 0;
}

.dummy-map {
  width: 100%;
  height: 20rem;
  background-color: dimgray;
  position: relative;
}

.dummy-text {
  position: absolute;
  text-align: center;
  color: white;
  top: 50%;
  left:50%;
  transform: translate(-50%, -50%);
}
</style>