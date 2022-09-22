<template>
  <v-app>
    <v-app-bar app elevation="2">Matkoncept</v-app-bar>
    <v-main>
      <v-container>
        <h2>Veckomeny</h2>
        <div>
          <v-col cols="4">
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
          </v-col>
        </div>
      </v-container>
      <v-container>
        <v-btn
          large
          @click="generate(chosenFoodMeat, chosenFoodFish, chosenFoodVeg)"
          >Generera</v-btn
        >

        <v-card v-if="dishCardShow" class="mx-auto" max-width="344">
          <v-card-title>
            <div>Veckans matsedel</div>
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
            <v-btn
              @click="dishCardShow = false"
              text
              color="deep-purple accent-4"
            >
              Stäng
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
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
  data() {
    return {
      chosenFoodMeat: false,
      dishes: [],
      dishCardShow: false,
      chosenFoodFish: false,
      chosenFoodVeg: false,
      week: [
        {
          id: "0",
          day: "Måndag",
        },
        {
          id: "1",
          day: "Tisdag",
        },
        {
          id: "2",
          day: "Onsdag",
        },
        {
          id: "3",
          day: "Torsdag",
        },
        {
          id: "4",
          day: "Fredag",
        },
        {
          id: "5",
          day: "Lördag",
        },
        {
          id: "6",
          day: "Söndag",
        },
      ],
    };
  },

  methods: {
    /* async created() {
      this.dishes = await this.fetchDishes();

    }, */

    async fetchDishes() {
      const res = await fetch("http://localhost:5000/dishes");
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
