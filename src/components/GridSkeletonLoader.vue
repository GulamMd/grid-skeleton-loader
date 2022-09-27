<template>
  <v-container>
    <v-card>
      <v-card-title class="py-3 text-center">
        Grid Skeleton Loader
      </v-card-title>
      <v-divider class="pa-0 ma-0"></v-divider>
      <v-card-actions class="table-toolbar">
        <span class="table-header ml-3">Dessert Ingridients Table</span>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          class="pb-4"
          label="Search"
          single-line
          hide-details
          clear-icon="mdi-close-circle"
          clearable
          @click:clear="
            () => {
              search = '';
              resetSearch();
            }
          "
          @keyup="searchInternal()"
        ></v-text-field>
        <v-btn
          icon
          class="mr-0"
          title="Refresh"
          :disabled="loading"
          @click="refreshTable"
        >
          <v-icon>mdi-refresh</v-icon>
        </v-btn>
      </v-card-actions>
      <v-divider class="pa-0 ma-0"></v-divider>
      <v-card-text>
        <v-row>
          <v-col
            v-for="item in headers"
            :key="`${item.text}+'E1'`"
            class="pl-4 mb-0"
          >
            <span>{{ item.text }}</span>
          </v-col>
        </v-row>
        <v-row
          v-for="rowItem in rows"
          :key="`${rowItem.name}+'A'`"
          class="each-row"
          v-show="!loading"
        >
          <v-col
            v-for="item in headers"
            :key="`${item.value}+'B'`"
            class="pl-4"
            >{{ rowItem[item.value] }}</v-col
          >
        </v-row>
        <v-row
          v-for="rowItem in rows"
          :key="`${rowItem.name}+'C'`"
          class="each-row"
          v-show="loading"
        >
          <v-col v-for="item in headers" :key="`${item.value}+'D'`">
            <v-skeleton-loader
              class="ml-1 mt-1"
              type="text"
            ></v-skeleton-loader>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>
<script>
export default {
  name: "GridSkeletonLoader",
  data() {
    return {
      loading: false,
      search: "",
      rows: [],
      headers: [
        {
          text: "Dessert (100g serving)",
          value: "name",
        },
        { text: "Calories", value: "calories" },
        { text: "Fat (g)", value: "fat" },
        { text: "Carbs (g)", value: "carbs" },
        { text: "Protein (g)", value: "protein" },
        { text: "Iron (%)", value: "iron" },
      ],
      desserts: [
        {
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%",
        },
        {
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%",
        },
        {
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%",
        },
        {
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%",
        },
        {
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%",
        },
        {
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%",
        },
        {
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%",
        },
        {
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%",
        },
        {
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%",
        },
        {
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%",
        },
      ],
    };
  },
  watch: {
    desserts: {
      handler(v) {
        this.rows = [];
        this.rows.push(...v);
      },
      immediate: true,
    },
  },
  methods: {
    refreshTable() {
      this.loading = true;
      setTimeout(() => {
        this.loading = false;
      }, 2000);
    },
    resetSearch() {
      this.searchInternal();
    },
    searchInternal() {
      let searchTxt = this.search.trim().toLowerCase();
      if (searchTxt === "") {
        this.rows = [];
        this.rows.push(...this.desserts);
        return;
      }
      let filterArr = this.rows.filter((item) => {
        return searchTxt
          .split(" ")
          .every((v) => item.name?.toLowerCase().includes(v));
      });
      this.rows = [];
      this.rows.push(...filterArr);
    },
  },
};
</script>
<style scoped>
.text-center {
  display: flex;
  justify-content: center;
}
.table-toolbar {
  display: flex;
}
.table-header {
  font-size: 16px;
  font-weight: 600;
  margin-top: 3px;
}
.each-row {
  border-top: 1px solid lightgrey;
}
</style>