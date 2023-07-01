<template>
  <div class="hello">
    <h1>Mini Aplikasi Klub Sepak Bola</h1>
    <div>
      <h2>Input Data Klub</h2>
      <form @submit.prevent="addClub">
        <label for="clubName">Nama Klub:</label>
        <input type="text" id="clubName" v-model="newClubName" required />
        <label for="clubCity">Kota Klub:</label>
        <input type="text" id="clubCity" v-model="newClubCity" required />
        <button type="submit">SAVE</button>
      </form>
    </div>
    <div>
      <h2>Input Skor Pertandingan</h2>
      <form @submit.prevent="addScore">
        <label for="homeClub">Klub Tuan Rumah:</label>
        <select id="homeClub" v-model="selectedHomeClub">
          <option v-for="club in clubs" :key="club.id" :value="club.id">
            {{ club.name }}
          </option>
        </select>
        <label for="awayClub">Klub Tamu:</label>
        <select id="awayClub" v-model="selectedAwayClub">
          <option v-for="club in clubs" :key="club.id" :value="club.id">
            {{ club.name }}
          </option>
        </select>
        <label for="homeScore">Skor Tuan Rumah:</label>
        <input type="number" id="homeScore" v-model="newHomeScore" required />
        <label for="awayScore">Skor Tamu:</label>
        <input type="number" id="awayScore" v-model="newAwayScore" required />
        <button type="submit">SAVE</button>
      </form>
    </div>

    <div>
      <h2>Klasemen</h2>
      <table class="center">
        <tr>
          <th>No.</th>
          <th>Nama Klub</th>
          <th>Kota Klub</th>
          <th>Poin</th>
        </tr>
        <tr v-for="(club, index) in clubs" :key="club.id">
          <td>{{ index + 1 }}</td>
          <td>{{ club.name }}</td>
          <td>{{ club.city }}</td>
          <td>{{ club.points }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function () {
    return {
      clubs: [],
      newClubName: "",
      newClubCity: "",
      selectedHomeClub: "",
      selectedAwayClub: "",
      newHomeScore: "",
      newAwayScore: "",
    };
  },
  methods: {
    addClub() {
      const clubExists = this.clubs.find(
        (club) => club.name.toLowerCase() === this.newClubName.toLowerCase()
      );
      if (clubExists) {
        alert("Nama klub sudah ada!");
        return;
      }

      const newClub = {
        id: Date.now(),
        name: this.newClubName,
        city: this.newClubCity,
        points: 0,
      };
      this.clubs.push(newClub);
      this.newClubName = "";
      this.newClubCity = "";
    },
    addScore() {
      const homeClub = this.clubs.find(
        (club) => club.id === this.selectedHomeClub
      );
      const awayClub = this.clubs.find(
        (club) => club.id === this.selectedAwayClub
      );
      if (!homeClub || !awayClub) {
        alert("Klub tidak valid!");
        return;
      }

      const homeScore = parseInt(this.newHomeScore);
      const awayScore = parseInt(this.newAwayScore);
      if (isNaN(homeScore) || isNaN(awayScore)) {
        alert("Skor harus berupa angka!");
        return;
      }

      homeClub.points +=
        homeScore > awayScore ? 3 : homeScore === awayScore ? 1 : 0;
      awayClub.points +=
        awayScore > homeScore ? 3 : awayScore === homeScore ? 1 : 0;

      this.newHomeScore = "";
      this.newAwayScore = "";
    },
  },
};
</script>

<style scoped>
/* h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
  text-align: center;
}
li {
  display: inline-block;
  margin: 0 10px; */
/* } */
a {
  color: #42b983;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
  width: 100%;
  text-align: center;
  border: 1px solid #ddd;
}

th {
  text-align: center;
  padding-left: 14px;
}
td {
  text-align: center;
  padding-left: 14px;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}
</style>
