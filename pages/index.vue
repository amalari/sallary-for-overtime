<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <v-img :src="`/logo.jpg`" height="250px"></v-img>
      </div>
      <v-card style="width:500px">
        <v-card-title class="headline">
          <v-flex xs8>Hitung Upah Lembur Anda</v-flex>
          <v-flex xs4 text-xs-right>
            <nuxt-link to="/rumus">
              <v-btn color="info">Rumus</v-btn>
            </nuxt-link>
          </v-flex>
        </v-card-title>
        <v-card-text>
          <template>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="sallary"
                :rules="sallaryRules"
                label="Gaji"
                required
              ></v-text-field>

              <v-text-field
                v-model="day"
                :rules="dayRules"
                label="Hari"
                required
              ></v-text-field>

              <v-select
                v-model="hoursOfWeek"
                :items="itemsHoursOfWeek"
                item-text="text"
                item-value="value"
                :rules="[v => !!v || 'Jam kerja harus diisi']"
                label="Jam Kerja"
                required
              >
              </v-select>
              <div class="text-xs-center">
                <v-btn :disabled="!valid" color="primary" @click="addItem">
                  Submit
                </v-btn>
                <v-btn color="error" @click="reset('form')">
                  Reset
                </v-btn>
              </div>
            </v-form>
          </template>
          <v-form
            v-if="parseInt(detailCounter) > 0"
            ref="form1"
            v-model="valid"
            lazy-validation
          >
            <v-container v-for="i in parseInt(detailCounter)" :key="i">
              <v-layout>
                <v-flex xs12 md6>
                  <v-text-field
                    v-model="detailHours[i].hours"
                    :rules="hoursRules"
                    label="Jam"
                    required
                  ></v-text-field>
                </v-flex>
                <v-flex xs12 md6>
                  <v-select
                    v-model="detailHours[i].dayType"
                    :items="dayTypes"
                    item-text="text"
                    item-value="value"
                    :rules="[v => !!v || 'Jam Kerja is required']"
                    label="Jenis Hari"
                    required
                  >
                  </v-select>
                </v-flex>
              </v-layout>
            </v-container>
            <div class="text-xs-center">
              <v-btn :disabled="!valid" color="primary" @click="sallaryCount">
                Hitung
              </v-btn>
              <v-btn color="error" @click="reset('form1')">
                Reset
              </v-btn>
            </div>
            <br />
            <br />
            <br />
            <h2 class="text-xs-center">{{ result }}</h2>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    sallary: '',
    sallaryRules: [
      v => !!v || 'Gaji harus diisi',
      v => (v && !isNaN(v)) || 'Gaji harus berisi angka'
    ],
    day: '',
    dayRules: [
      v => !!v || 'Hari harus diisi',
      v => (v && !isNaN(v)) || 'Hari harus berisi angka'
    ],
    hoursOfWeek: null,
    itemsHoursOfWeek: [
      {
        value: 1,
        text: '5 Hari (40 Jam/Minggu)'
      },
      {
        value: 2,
        text: '6 Hari (40 Jam/Minggu)'
      }
    ],
    detailHours: [],
    detailCounter: 0,
    dayTypes: [
      {
        value: 1,
        text: 'Hari Biasa'
      },
      {
        value: 2,
        text: 'Hari Libur'
      },
      {
        value: 3,
        text: 'Hari Libur Terpendek'
      }
    ],
    hoursRules: [
      v => !!v || 'Jam harus diisi',
      v => (v && !isNaN(v) && parseInt(v) <= 24) || 'Jam harus berisi angka'
    ],
    constantaUpah: 1 / 173,
    result: 0
  }),

  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true
      }
    },
    addItem() {
      if (this.$refs.form.validate()) {
        for (let index = 1; index <= parseInt(this.day); index++) {
          this.detailHours[index] = {
            hours: '',
            dayType: null
          }
        }
        this.detailCounter = this.day
      }
    },
    reset(param) {
      this.$refs[param].reset()
      if (param === 'form1') {
        this.detailCounter = 0
        this.result = 0
      }
    },
    sallaryCount() {
      this.detailHours.forEach(detailHour => {
        if (detailHour.dayType === 1) {
          for (let hour = 1; hour <= parseInt(detailHour.hours); hour++) {
            if (hour === 1) {
              this.result += parseInt(this.sallary) * 1 * 1.5 * (1 / 173)
            } else {
              this.result += parseInt(this.sallary) * 1 * 2 * (1 / 173)
            }
          }
        } else if (detailHour.dayType === 2) {
          if (this.hoursOfWeek === 1) {
            for (let hour = 1; hour <= parseInt(detailHour.hours); hour++) {
              if (hour <= 8) {
                this.result += parseInt(this.sallary) * 1 * 2 * (1 / 173)
              } else {
                this.result += parseInt(this.sallary) * 1 * 3 * (1 / 173)
              }
            }
          } else {
            for (let hour = 1; hour <= parseInt(detailHour.hours); hour++) {
              if (hour <= 8) {
                this.result += parseInt(this.sallary) * 1 * 2 * (1 / 173)
              } else if (hour === 9) {
                this.result += parseInt(this.sallary) * 1 * 3 * (1 / 173)
              } else {
                this.result += parseInt(this.sallary) * 1 * 4 * (1 / 173)
              }
            }
          }
        } else {
          for (let hour = 1; hour <= parseInt(detailHour.hours); hour++) {
            if (hour <= 5) {
              this.result += parseInt(this.sallary) * 1 * 2 * (1 / 173)
            } else if (hour === 6) {
              this.result += parseInt(this.sallary) * 1 * 3 * (1 / 173)
            } else {
              this.result += parseInt(this.sallary) * 1 * 4 * (1 / 173)
            }
          }
        }
      })

      this.result = Math.round(this.result / 100) * 100
    },
    resetValidation() {
      this.$refs.form.resetValidation()
    }
  }
}
</script>
