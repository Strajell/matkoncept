<template>
  <v-app>
    <v-app-bar app elevation="2">Matkoncept</v-app-bar>
    <v-main>
      <v-container>
        <h2>Veckomeny</h2>
        <div>
          <v-row
            ><v-col cols="3">
              <div>
                <FoodButton
                  @btn-click="
                    // eslint-disable-next-line vue/no-mutating-props
                    chosenFoodMeat = !chosenFoodMeat
                  "
                  :color="chosenFoodMeat ? 'green' : ''"
                  :buttontext="'Kött'"
                />
                <FoodButton
                  @btn-click="
                    // eslint-disable-next-line vue/no-mutating-props
                    chosenFoodFish = !chosenFoodFish
                  "
                  :color="chosenFoodFish ? 'green' : ''"
                  :buttontext="'Fisk'"
                />
                <FoodButton
                  @btn-click="
                    // eslint-disable-next-line vue/no-mutating-props
                    chosenFoodVeg = !chosenFoodVeg
                  "
                  :color="chosenFoodVeg ? 'green' : ''"
                  :buttontext="'Veg'"
                />
              </div>
              <br />
              <v-btn
                large
                @click="generate(chosenFoodMeat, chosenFoodFish, chosenFoodVeg)"
                >Generera</v-btn
              >
            </v-col>
            <v-col cols="3"
              ><v-card class="mx-auto" max-width="344">
                <v-card-title>
                  <div id="cardtitle">Veckans matsedel</div>
                </v-card-title>

                <v-card-text>
                  <li v-for="day in week" :key="day.id">
                    {{ day.day }}:
                    {{
                      makeWeekMenu(
                        dishes,
                        chosenFoodMeat,
                        chosenFoodFish,
                        chosenFoodVeg
                      ).name
                    }}
                  </li>
                </v-card-text>
                <v-card-actions>
                  <v-btn text color="deep-purple accent-4"> Recept </v-btn>
                </v-card-actions>
              </v-card>
            </v-col></v-row
          >
        </div>
      </v-container>
      <v-container> </v-container>
    </v-main>
  </v-app>
</template>

<script>
import FoodButton from "./components/FoodButton";

export default {
  name: "App",
  props: {
    title: String,
  },
  async mounted() {
    const dishes = [];
    const week = [];
    this.dishes = await this.fetchDishes();
    this.week = await this.fetchWeek();
    return dishes, week;
  },
  data() {
    return {
      chosenFoodMeat: false,
      dishes: [],
      dishCardShow: false,
      chosenFoodFish: false,
      chosenFoodVeg: false,
      week: [],
    };
  },

  methods: {
    async fetchDishes() {
      const res = await fetch("http://localhost:5000/dishes");
      const data = await res.json();
      return data;
    },

    async fetchWeek() {
      const res = await fetch("http://localhost:5000/week");
      const data = await res.json();

      return data;
    },

    async generate() {
      this.dishes = await this.fetchDishes();
      this.dishCardShow = true;
    },
    makeWeekMenu(dishes, chosenFoodMeat, chosenFoodFish, chosenFoodVeg) {
      !chosenFoodMeat && !chosenFoodFish && !chosenFoodVeg
        ? ((chosenFoodMeat = !chosenFoodMeat),
          (chosenFoodFish = !chosenFoodFish),
          (chosenFoodVeg = !chosenFoodVeg))
        : console.log();

      const avaliableDishes = dishes.filter(
        (dish) =>
          dish.meat === chosenFoodMeat ||
          dish.fish === chosenFoodFish ||
          dish.veg === chosenFoodVeg
      );

      const random = Math.floor(Math.random() * avaliableDishes.length);
      const randomDish = avaliableDishes[random];
      return randomDish;
    },
  },
  components: {
    // eslint-disable-next-line vue/no-unused-components
    FoodButton,
  },
};
</script>

<style>
.v-btn {
  margin: 10px;
}
h2 {
  font-family: "Lato", sans-serif;
  font-size: 54px;
  font-weight: 300;
  line-height: 58px;
  margin: 0 0 15px;
}
#cardtitle {
  font-family: "Lato", sans-serif;
  font-size: 26px;
  line-height: 26px;
  margin: 0 0 15px;
}
</style>
