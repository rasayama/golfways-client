<template>
  <div>
    <v-form
      class="route-search-form"
      ref="route_search_form"
      v-model="route_search_valid"
      lazy-validation
    >
      <v-text-field
        class="body-2"
        v-model="name"
        :rules="nameRules"
        outlined
        dense
        hide-details
        label="出発地点"
        prepend-icon="mdi-circle-double"
      />
      <v-select
        class="body-2"
        v-model="select"
        :items="items"
        outlined
        dense
        hide-details
        label="ゴルフ場名"
        prepend-icon="mdi-map-marker"
      />
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

    <v-form
      class="price-calc-form"
      ref="price_calc_form"
      v-model="price_calc_valid"
      lazy-validation
    >
      <v-row>
        <v-col cols="5">
          <v-dialog
            ref="calendar_modal"
            v-model="calendar_modal"
            :return-value.sync="date"
            width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                class="body-2"
                v-model="dateFormatted"
                prepend-icon="mdi-calendar"
                readonly
                v-on="on"
              />
            </template>
            <v-date-picker
              v-model="date"
              scrollable
              locale="ja"
              header-color="green"
              @input="calendar_modal = false, $refs.calendar_modal.save(date)"
            />
          </v-dialog>
        </v-col>

        <v-col cols="3">
          <v-dialog
            ref="clock_modal"
            v-model="clock_modal"
            :return-value.sync="time"
            width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                class="body-2 mx-2"
                v-model="time"
                prepend-icon="mdi-clock"
                readonly
                v-on="on"
              />
            </template>
            <v-time-picker
              v-if="clock_modal"
              v-model="time"
              header-color="green"
              @click:minute="$refs.clock_modal.save(time)"
            />
          </v-dialog>
        </v-col>

        <v-col cols="3" class="add-list-button">
          <v-btn rounded dark color="green" class="mx-4">
            <v-icon>mdi-plus</v-icon>
              LIST
          </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </div>
</template>

<script>
  const weekday = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
  export default {
    data: vm => ({
      route_search_valid: true,
      price_calc_valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters',
      ],
      select: null,
      items: [
        'Golf Course 1',
        'Golf Course 2',
        'Golf Course 3',
      ],
      date: new Date().toISOString().substr(0, 10),
      dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
      time: null,
      calendar_modal: false,
      clock_modal: false,
    }),

    computed: {
      computedDateFormatted () {
        return this.formatDate(this.date)
      }
    },

    watch: {
      date () {
        this.dateFormatted = this.formatDate(this.date)
      }
    },

    methods: {
      validate () {
        if (this.$refs.form.validate()) {
          this.snackbar = true
        }
      },
      reset () {
        this.$refs.form.reset()
      },
      formatDate (date) {
        if (!date) return null
        const [year, month, day] = date.split('-')
        return `${year}/${month}/${day}(${weekday[new Date(date).getDay()]})`
      },
    },
  }
</script>

<style scoped>
form {
  width: 90%;
}

.route-search-form {
  margin: 15px auto;
}

.route-search-form .v-input + .v-input {
  margin-top: 15px;
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

.price-calc-form {
  margin: 0 auto;
}

.price-calc-form .row{
  margin: 0;
}

.price-calc-form .col {
  padding: 0;
}

.add-list-button button {
  margin-top: 13px;
}
</style>