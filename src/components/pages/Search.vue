<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="10"
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

    <span style="float: right;">
      <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      検索
    </v-btn>

    <v-btn
      color="error"
      class="mr-4"
      @click="reset"
    >
      検索条件をリセット
    </v-btn>
    </span>
  </v-form>
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
        'Item 1',
        'Item 2',
        'Item 3',
        'Item 4',
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
</style>