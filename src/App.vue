<template>
  <div class="px-3">
    <nav class="p-4 font-medium">
      <h1>Habito {{ activeYear }}</h1>
    </nav>

    <div class="text-xs flex gap-1 overflow-auto p-2">
      <button
        class="px-3 py-1 transition-colors rounded-md hover:bg-slate-50"
        :class="{
          'bg-slate-50 ring-1 ring-slate-300': activeMonth.id === m.id,
        }"
        v-for="m in months"
        @click="clickMonth(m)"
      >
        {{ m.name }}
      </button>
    </div>

    <h2>HABIT</h2>
    <div class="flex">
      <div class="flex flex-col w-40">
        <input
          v-for="h in habits"
          v-model="habits[h.id].name"
          class="habit"
          type="text"
        />
        <button
          class="px-3 py-2 bg-slate-100 text-slate-500 hover:text-slate-800"
          @click="handleAddHabit"
        >
          Add habit
        </button>
      </div>
      <div class="flex flex-col flex-grow overflow-auto">
        <div v-for="row in habits" class="inline-flex">
          <input
            v-for="item in activeMonth.days"
            @change="handleChange(row, item)"
            :checked="itemIsActive(row, item)"
            :placeholder="item"
            type="checkbox"
          />
        </div>
      </div>
    </div>

    <pre class="bg-slate-800 text-slate-200 p-4 rounded-lg m-4 mt-24">{{
      habits
    }}</pre>
  </div>
</template>
<script>
export default {
  data() {
    return {
      activeMonth: undefined,
      activeYear: undefined,
      habits: {
        0: {
          id: 0,
          name: "Eat healthy",
          history: {
            2022: {
              0: [1, 3, 8, 12],
              1: [],
              2: [],
              3: [],
              4: [],
              5: [],
              6: [],
              7: [],
              8: [],
              9: [],
              10: [],
              11: [],
            },
          },
        },
        1: {
          id: 1,
          name: "Exercise",
          history: {
            2022: {
              0: [1, 2, 3, 4],
              1: [],
              2: [],
              3: [],
              4: [],
              5: [],
              6: [],
              7: [],
              8: [],
              9: [],
              10: [],
              11: [],
            },
          },
        },
      },
    };
  },
  methods: {
    clickMonth(month) {
      this.activeMonth = month;
    },
    handleAddHabit() {
      const id = Object.keys(this.habits).length;
      this.habits[id] = {
        id,
        name: "",
        history: {
          2022: {
            0: [],
            1: [],
            2: [],
            3: [],
            4: [],
            5: [],
            6: [],
            7: [],
            8: [],
            9: [],
            10: [],
            11: [],
          },
        },
      };
    },
    handleChange(row, item) {
      const habit = this.habits[row.id];
      const index =
        habit.history[this.activeYear]?.[this.activeMonth.id]?.indexOf(item);

      if (index === -1) {
        habit.history[this.activeYear][this.activeMonth.id].push(item);
      } else {
        habit.history[this.activeYear][this.activeMonth.id].splice(index, 1);
      }

      console.log(index, habit.name, item);
    },
    itemIsActive(row, item) {
      const habit = this.habits[row.id];
      return habit.history[this.activeYear]?.[this.activeMonth.id]?.includes(
        item
      );
    },
  },
  created() {
    const currentMonth = this.months[new Date().getMonth()];
    const currentYear = new Date().getFullYear();
    this.activeMonth = currentMonth;
    this.activeYear = currentYear;
  },
  computed: {
    months() {
      return {
        0: {
          id: 0,
          name: "January",
          days: 31,
        },
        1: {
          id: 1,
          name: "February",
          days: 28,
        },
        2: {
          id: 2,
          name: "March",
          days: 31,
        },
        3: {
          id: 3,
          name: "April",
          days: 30,
        },
        4: {
          id: 4,
          name: "May",
          days: 31,
        },
        5: {
          id: 5,
          name: "June",
          days: 30,
        },
        6: {
          id: 6,
          name: "July",
          days: 31,
        },
        7: {
          id: 7,
          name: "August",
          days: 31,
        },
        8: {
          id: 8,
          name: "September",
          days: 30,
        },
        9: {
          id: 9,
          name: "October",
          days: 31,
        },
        10: {
          id: 10,
          name: "November",
          days: 30,
        },
        11: {
          id: 11,
          name: "December",
          days: 31,
        },
      };
    },
  },
};
</script>
<style>
.habit {
  @apply ring-1 ring-inset ring-slate-300 px-2 h-8 w-40 hover:ring-blue-300 focus:ring-blue-500 transition-all;
}

input[type="checkbox"] {
  appearance: none;
  @apply border w-8 h-8;
  display: grid;
  place-content: center;
}
input[type="checkbox"]::before {
  content: "";
  width: 1rem;
  height: 1rem;
  transform: scale(0);
  transition: 120ms transform ease-in-out;
  box-shadow: inset 1em 1em theme("colors.blue.500");
}
input[type="checkbox"]:checked::before {
  transform: scale(1);
}
</style>
