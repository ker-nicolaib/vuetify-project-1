<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto"
                  max-width="900">

      <h1 class="my-2">List of Persons</h1>

      <v-text-field class="my-2" label="Search" v-model="search"></v-text-field>
      <v-infinite-scroll :height="300" :items="items" :onLoad="load">
        <template v-for="(item, index) in items" :key="item">
          <div :class="['pa-2', index % 2 === 0 ? 'bg-grey-lighten-2' : '']">
            Person: {{ item }}
          </div>
        </template>
      </v-infinite-scroll>

    </v-responsive>
  </v-container>
</template>

<script setup>
  import { ref, watch } from 'vue'

  const search = ref('')

  const items = ref([
    'Nick',
    'Alex',
    'Jane',
    'Mary',
    'Julia',
    'Zoe',
    'Laura',
  ])

  const names = ref([
    'John',
    'Simon',
    'Stephen',
    'Max',
    'Rick',
    'Betty',
    'Tom',
    'Brad',
    'Mark',
    'Elizabeth',
  ])

  async function api() {
    return new Promise(resolve => {
      setTimeout(() => {
        const array = []
        for (let i = 0; i < 10; i++) {
          const name = names.value[Math.floor(Math.random() * 10)]
          if (name.includes(search.value)) {
            array.push(name)
          }
        }

        resolve(array)
      }, 1000)
    })
  }
  async function load({ done }) {
    // Perform API call
    const res = await api()

    if (res.length < 1) {
      done('empty')
    } else {
      items.value.push(...res)
      done('ok')
    }
  }
</script>
