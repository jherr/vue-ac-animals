<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-toolbar-title>Animal Crossing Animals</v-toolbar-title>
      <v-spacer></v-spacer>  
      <div class="search-field">
        <v-text-field
          label="Search"
          single-line
          outlined
          dense
          v-model="search"
        ></v-text-field>
      </div>
    </v-app-bar>

    <v-content>
      <v-data-table
        :headers="headers"
        :items="animals"
        :items-per-page="50"
        class="elevation-1"
      >
        <template v-slot:item.name="{ item }">
          <strong>{{item.name}}</strong>
        </template>
        <template v-slot:item.priceValue="{ item }">
          {{item.price}}
        </template>
        <template v-slot:item.image="{ item }">
          <img :src="item.image" />
        </template>
      </v-data-table>
    </v-content>
  </v-app>
</template>

<script>
import animals from './animals.json';

const animalTypes = {
  bug: 'Bug',
  deepSeaCreature: 'Deep Sea Creature',
  fish: 'Fish',
};

const months = [
  'jan',
  'feb',
  'mar',
  'apr',
  'may',
  'jun',
  'jul',
  'aug',
  'sep',
  'oct',
  'nov',
  'dec',
];
const monthNames = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
  'July',
  'August',
  'September',
  'October',
  'November',
  'December',
];

export default {
  name: 'App',

  data: () => ({
    search: '',
    headers: [
      {
        text: 'Animal',
        align: 'start',
        sortable: true,
        value: 'name',
      },
      {
        text: 'Image',
        value: 'image',
      },
      {
        text: 'Type',
        align: 'start',
        sortable: true,
        value: 'type',
      },      
      {
        text: 'Price',
        align: 'start',
        sortable: true,
        value: 'priceValue',
      },      
      {
        text: 'Months',
        sortable: false,
        value: 'monthText',
      },      
    ],
  }),
  computed: {
    animals() {
      const monthSegments = (monthList) => {
        const segments = [];
        let start = -1;
        months.forEach((m, i) => {
          if (monthList.includes(m)) {
            if (start === -1) {
              start = i;
            }
          } else {
            if (start != -1) {
              segments.push({
                start, 
                end: i - 1
              });
            }
            start = -1;
          }
        });
        if (start != -1) {
          segments.push({
            start, 
            end: months.length - 1,
          });
        }
        return segments
          .map(({ start, end }) => `${monthNames[start]}-${monthNames[end]}`)
          .join(', ');
      };

      const searchLower = this.search.toLowerCase();
      return animals
        .filter(({name}) => name.toLowerCase().includes(searchLower))
        .map((animal) => ({
          ...animal,
          monthText: monthSegments(animal.months),
          priceValue: parseInt(animal.price.replace(/,/g, '')),
          type: animalTypes[animal.animalType],
        }));
    }
  }
};
</script>

<style scoped>
.search-field {
  padding-top: 15px;
}
</style>