<template>
  <div>
    <h2>Här kommer veckomenyn</h2>

    <!-- <li v-for="day in week" :key="day.id">
      {{ day.day }}:
      {{
        makeWeekMenu(dishes, chosenFoodMeat, chosenFoodFish, chosenFoodVeg).name
      }}
    </li> -->
  </div>
</template>

<script>
export default {
  name: "WeekChoice",
  props: {},
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
    created() {
      this.dishes = this.fetchDishes();

      console.log(this.dishes);
    },

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
      console.log(dishes);
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
};
</script>
