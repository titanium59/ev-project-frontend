<template>
  <div v-if="station" class="station-container">
    <h2>{{ station.name }}</h2>
    <p><strong>Location:</strong> {{ station.location.coordinates.join(', ') }}</p>
    <p><strong>Status:</strong> {{ station.status }}</p>
    <p><strong>Power Output:</strong> {{ station.powerOutput }} kW</p>
    <p><strong>Connector Type:</strong> {{ station.connectorType }}</p>
    <button
      v-if="station.owner && station.owner._id === loggedInOwnerId"
      @click="goToEdit"
      class="edit-btn"
    >Edit</button>
    <button @click="goBack" class="back-btn">Back</button>
  </div>
  <div v-else class="loading-container">
    <p>Loading...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      station: null,
      loggedInOwnerId: localStorage.getItem('ownerId')
    }
  },
  mounted() {
    fetch(`${process.env.VUE_APP_API_URL}/stations/${this.$route.params.id}`)
      .then(res => res.json())
      .then(data => {
        this.station = data
      })
  },
  methods: {
    goToEdit() {
      this.$router.push(`/stations/${this.$route.params.id}/edit`);
    },
    goBack() {
      this.$router.push('/');
    }
  }
}
</script>

<style scoped>
.station-container {
  max-width: 500px;
  margin: 40px auto;
  padding: 32px 24px;
  background: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.07);
  text-align: left;
}
.station-container h2 {
  margin-bottom: 18px;
  color: #2c3e50;
}
.station-container p {
  margin: 10px 0;
  font-size: 1.08em;
}
.edit-btn, .back-btn {
  margin-top: 18px;
  margin-right: 10px;
  padding: 8px 22px;
  border: none;
  border-radius: 5px;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.2s;
}
.edit-btn {
  background: #1976d2;
  color: #fff;
}
.edit-btn:hover {
  background: #125ea2;
}
.back-btn {
  background: #e0e0e0;
  color: #333;
}
.back-btn:hover {
  background: #bdbdbd;
}
.loading-container {
  text-align: center;
  margin-top: 60px;
  font-size: 1.2em;
  color: #888;
}
</style>