<template>
  <main class="d-flex flex-column pa-4">
    <v-text-field
        v-model="newItemTitle"
        label="New Item"
        append-icon="mdi-plus"
        clearable
        @click:append="addNewItem"
        @keyup.enter="addNewItem"
    ></v-text-field>

    <v-list class="flex-grow-1" select-strategy="leaf">
      <div v-for="item in shoppingItems" :key="item.id">
        <v-list-item
            :value="item.bought"
            :class="{ 'blue lighten-5': item.bought }"
            class="d-flex align-center"
            @click="doneBought(item.id)"
        >
          <template v-slot:prepend>
            <v-list-item-action start>
              <v-checkbox-btn
                  :model-value="item.bought"
                  @click.stop="doneBought(item.id)"
              />
            </v-list-item-action>
          </template>

          <v-list-item-title
              :class="{ 'text-decoration-line-through': item.bought }"
              class="flex-grow-1"
          >
            {{ item.title }}
          </v-list-item-title>

          <template v-slot:append>
            <v-list-item-action>
              <v-btn icon @click="deleteItem(item.id)">
                <i class="mdi mdi-delete"></i>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>

        <v-divider></v-divider>
      </div>
    </v-list>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const shoppingItems = ref([
  { id: 1, title: 'Milk', bought: false },
  { id: 2, title: 'Bread', bought: false },
  { id: 3, title: 'Eggs', bought: false }
])

const newItemTitle = ref('')

function doneBought(id: number) {
  const item = shoppingItems.value.find(i => i.id === id)
  if (item) {
    item.bought = !item.bought
  }
}

function deleteItem(id: number) {
  shoppingItems.value = shoppingItems.value.filter(item => item.id !== id)
}

function addNewItem() {
  if (newItemTitle.value.trim()) {
    shoppingItems.value.push({
      id: shoppingItems.value.length + 1,
      title: newItemTitle.value,
      bought: false
    })
    newItemTitle.value = ''
  }
}
</script>
