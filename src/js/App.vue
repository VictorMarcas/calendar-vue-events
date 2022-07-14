<template>
  <div class="flex items-center justify-center w-full h-screen bg-gray-300">
    <div>
      <div class="flex w-full max-w-screen-md gap-8">
        <div class="flex-none">
          <h3 class="mb-4 text-base font-bold text-gray-700">
            1. Selecciona una fecha:
          </h3>
          <vc-date-picker
            mode="date"
            locale="es"
            color="indigo"
            :min-date="nextDate"
            :max-date="finishDate"
            dayclick="dayClick"
            title-position="left"
            v-model="selectedDate"
          />
        </div>
        <div class="flex flex-col flex-1">
          <h3 class="flex-none mb-4 text-base font-bold text-gray-700">
            2. Selecciona la hora:
          </h3>
          <form class="flex flex-col flex-1">
            <div class="flex flex-wrap gap-x-3 gap-y-4">
              <label
                :for="`hour${index}`"
                v-for="(hour, index) in hoursAvailable"
                :key="index"
              >
                <input
                  type="radio"
                  :id="`hour${index}`"
                  name="hours"
                  :value="hour"
                  v-model="selectedHour"
                  class="hidden [&:checked~span]:text-white [&:checked~span]:border-indigo-500 [&:checked~span]:bg-indigo-500"
                />
                <span
                  class="w-12 h-6 px-2 py-1.5 transition-colors duration-200 text-sm font-bold leading-6 border-2 rounded-md cursor-pointer bg-indigo-500/10 text-indigo-500 border-indigo-500"
                  >{{ hour }}</span
                >
              </label>
            </div>
            <div class="mt-auto">
              <h3 class="flex-none mb-4 text-base font-bold text-gray-700">
                3. Selecciona Categoría:
              </h3>
              <select
                name="categories"
                class="w-full px-3 py-3 text-sm leading-6 bg-white border-gray-600 rounded-lg focus:outline-none focus:ring-0 disabled:opacity-50 disabled:cursor-default"
                v-model="selectedCategory"
                id=""
                :disabled="selectedHour === null"
              >
                <option value="selected">Selecciona Categoría</option>
                <option
                  :value="category"
                  v-for="category in categoriesAvailable"
                  :key="category"
                >
                  {{ category }}
                </option>
              </select>
              <button
                type="button"
                class="w-full px-3 py-3 mt-4 text-sm font-bold leading-6 tracking-wide text-white uppercase bg-indigo-500 rounded-lg disabled:bg-gray-400"
                :disabled="disabledCartButton"
                @click="addToCart"
              >
                Comprar
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import VCalendar from "v-calendar";
Vue.use(VCalendar, {
  componentPrefix: "vc",
});
export default {
  name: "app",
  data() {
    return {
      message: "Hola Mundo",
      selectedDate: null,
      today: new Date(),
      hoursAvailable: [],
      categoriesAvailable: [],
      selectedHour: null,
      selectedCategory: "selected",
    };
  },
  watch: {
    selectedDate(newDate) {
      if (newDate) {
        this.getHours(newDate);
      } else {
        this.hoursAvailable = [];
      }
    },
    selectedHour(newHour) {
      if (newHour) {
        this.getOptionsCategory(newHour);
      } else {
        this.categoriesAvailable = [];
      }
    },
  },
  methods: {
    clearEventDay() {
      this.hour = null;
    },
    getHours(newDate) {
      this.hoursAvailable = [
        "08:00",
        "09:00",
        "10:00",
        "11:00",
        "12:00",
        "13:00",
        "14:00",
        "15:00",
        "16:00",
        "17:00",
      ];
    },
    getOptionsCategory() {
      this.categoriesAvailable = [
        "Categoría 1",
        "Categoría 2",
        "Categoría 3",
        "Categoría 4",
        "Categoría 5",
      ];
    },
    addToCart() {
      const data = {
        date: this.selectedDate,
        hour: this.selectedHour,
        category: this.selectedCategory,
      };
      window.console.log(data);
    },
  },
  computed: {
    nextDate() {
      return this.today.setDate(this.today.getDate() + 1);
    },
    finishDate() {
      return this.today.setDate(this.today.getDate() + 7);
    },
    disabledCartButton() {
      return this.selectedHour === null || this.selectedCategory === "selected";
    },
  },
  mounted() {
    this.selectedDate = this.nextDate;
  },
};
</script>
