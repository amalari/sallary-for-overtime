<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <v-img :src="`/formula.jpg`" height="250px"></v-img>
      </div>
      <v-card style="width:500px">
        <v-card-title class="headline">
          <v-flex xs8>Rumus</v-flex>
          <v-flex xs4 text-xs-right>
            <nuxt-link to="/">
              <v-btn color="info">Hitung</v-btn>
            </nuxt-link>
          </v-flex>
        </v-card-title>
        <v-card-text>
          <v-select
            v-model="day"
            :items="workDays"
            item-text="text"
            item-value="value"
            label="Hari Kerja"
            required
          >
          </v-select>
          <table v-if="day === 1" style="width:100%">
            <tr>
              <th>Keterangan</th>
              <th>Perhitungan</th>
            </tr>
            <tr>
              <td>Jam Pertama</td>
              <td>Gaji*1*1.5*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 2</td>
              <td>Gaji*jam lembur*2*(1/173)</td>
            </tr>
          </table>

          <br />
          <table v-if="day == 2" style="width:100%">
            <tr>
              <th>Keterangan</th>
              <th>Perhitungan</th>
            </tr>
            <tr>
              <td>7 Jam Pertama</td>
              <td>Gaji*Jam Lembur*2*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 8</td>
              <td>Gaji*jam lembur*3*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 9</td>
              <td>Gaji*jam lembur*4*(1/173)</td>
            </tr>
          </table>

          <br />
          <table v-if="day === 3" style="width:100%">
            <tr>
              <th>Keterangan</th>
              <th>Perhitungan</th>
            </tr>
            <tr>
              <td>8 Jam Pertama</td>
              <td>Gaji*Jam Lembur*2*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 9</td>
              <td>Gaji*jam lembur*3*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 10</td>
              <td>Gaji*jam lembur*4*(1/173)</td>
            </tr>
          </table>

          <br />
          <table v-if="day === 4" style="width:100%">
            <tr>
              <th>Keterangan</th>
              <th>Perhitungan</th>
            </tr>
            <tr>
              <td>5 Jam Pertama</td>
              <td>Gaji*Jam Lembur*2*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 6</td>
              <td>Gaji*jam lembur*3*(1/173)</td>
            </tr>
            <tr>
              <td>Jam Ke 7</td>
              <td>Gaji*jam lembur*4*(1/173)</td>
            </tr>
          </table>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<style scoped>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}

th,
td {
  padding: 5px;
  text-align: left;
}
</style>

<script>
export default {
  data: () => ({
    day: null,
    workDays: [
      {
        value: 1,
        text: 'Hari Biasa'
      },
      {
        value: 2,
        text: 'Hari Libur 5 Hari Kerja'
      },
      {
        value: 3,
        text: 'Hari Libur 6 Hari Kerja'
      },
      {
        value: 4,
        text: "Hari Libur Hari Terpendek (Jum'at)"
      }
    ]
  })
}
</script>
