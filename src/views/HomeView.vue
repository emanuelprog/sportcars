<template>
  <v-container>
    <v-row class="text-center">
        <v-col cols="12">
            <h1>Advertise your car!</h1>
        </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="8" md="3" v-for="car in cars" :key="car.id">
        <v-card class="mx-auto" max-width="344" max-height="500">
          <v-carousel
            width="344"
            height="200"
            hide-delimiter-background
            delimiter-icon="mdi-minus"
          >
            <v-carousel-item
              v-for="(image, imageIndex) in car.url"
              :key="imageIndex"
              reverse-transition="fade-transition"
              transition="fade-transition"
            >
              <v-img
                :src="image.src"
                style="width: 344px; height: 200px; object-fit: cover"
              ></v-img>
            </v-carousel-item>
          </v-carousel>
          <div class="pt-2 pl-2" style="position: absolute; top: 0; left: 0;">
            <v-chip v-if="car.offer" color="red" text-color="white" style="box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);">
              Featured offer!
            </v-chip>
          </div>
          <v-card-title> {{ car.model }} </v-card-title>
          <v-card-subtitle> {{ car.description }} </v-card-subtitle>
          <div class="d-flex">
            <v-card-subtitle style="font-size: 12px">
              {{ car.year }} / {{ car.year2 }}</v-card-subtitle
            >
            <v-spacer></v-spacer>
            <v-card-subtitle class="text-right" style="font-size: 12px"
              >{{ Number(car.km).toLocaleString() }}km</v-card-subtitle
            >
          </div>
          <div class="pa-3 text-center">
            <v-btn dark width="90%">Ver parcelas</v-btn>
          </div>
          <v-divider></v-divider>
          <div class="d-flex align-center ml-3">
            <v-icon>mdi-map-marker</v-icon>
            <v-card-subtitle class="mb-0 pl-1" style="font-size: 12px">
              {{ car.city }} - {{ car.state }}
            </v-card-subtitle>
            <v-spacer></v-spacer>
            <v-icon
              class="mr-3"
              :color="car.favorite ? 'red' : ''"
              @click="toggleFavorite(car)"
              >mdi-heart</v-icon
            >
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  filters: {
    formatUSD(value) {
      if (!value) return "";
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(value);
    },
  },
  data: () => ({
    cars: null,
    car_id: null,
  }),
  methods: {
    async getCars() {
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

      const req = await fetch(apiUrl);

      const data = await req.json();

      this.cars = data;

      this.cars.sort((a, b) => {
      if (a.offer && !b.offer) {
        return -1;
      }
      if (!a.offer && b.offer) {
        return 1;
      }
      return 0;
    });
    },
    async toggleFavorite(car) {
      car.favorite = !car.favorite;

      let apiUrl = "";

      if (
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
          navigator.userAgent
        )
      ) {
        apiUrl = "http://10.4.0.74:3000/cars/" + car.id;
      } else {
        apiUrl = "http://localhost:3000/cars/" + car.id;
      }

      try {
        const response = await fetch(apiUrl, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(car),
        });

        const updatedCar = await response.json();
      } catch (error) {
        car.favorite = !car.favorite;
      }
    },
  },
  mounted() {
    this.getCars();
  },
};
</script>