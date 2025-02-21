<script setup>
// Reaktiva referenser gör att Vue automatiskt uppdaterar DOM när data förändras
import { ref } from "vue";

// props
defineProps({
  changeButtonText: {
    type: String,
    default: "Edit",
  },
});

//editing är en reaktiv variabel som håller koll på om användaren vill se receptlistan (true) eller ej (false).
const editing = ref(false);

const daysOfWeek = [
  "Måndag",
  "Tisdag",
  "Onsdag",
  "Torsdag",
  "Fredag",
  "Lördag",
  "Söndag",
];

const meals = ["Frukost", "Lunch", "Middag"];

//  ref gör att ändringar i schemat reflekteras i användargränssnittet.
const weeklySchedule = ref({
  Måndag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Tisdag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Onsdag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Torsdag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Fredag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Lördag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
  Söndag: {
    Frukost: "",
    Lunch: "",
    Middag: "",
  },
});

// Tillgängliga recept
const recipes = ref([
  {
    id: 1,
    name: "Gröt",
    highPriority: false,
  },
  {
    id: 2,
    name: "Kokta ägg",
    highPriority: true,
  },
  {
    id: 3,
    name: "Pannkakor",
    highPriority: false,
  },
  {
    id: 4,
    name: "Pasta Bolognese",
    highPriority: true,
  },
  {
    id: 5,
    name: "Hamburgare",
    highPriority: false,
  },
  {
    id: 6,
    name: "Pizza",
    highPriority: false,
  },
]);

// Nya recept
const newRecipeName = ref("");

//Lägg till prioritering
const newRecipeHighPriority = ref(false);

// Lägg till nytt recept
const addNewRecipe = () => {
  recipes.value.push({
    id: recipes.value.length + 1,
    name: newRecipeName.value,
    highPriority: newRecipeHighPriority.value,
  });

  // Rensa inputfältet
  newRecipeName.value = "";
  newRecipeHighPriority.value = false;
};

// Spara recept till en viss dag och måltid
const saveRecipeToSchedule = (day, meal, recipeId) => {
  const selectedRecipe = recipes.value.find(
    (recipe) => recipe.id === parseInt(recipeId)
  );
  if (selectedRecipe) {
    weeklySchedule.value[day][meal] = selectedRecipe.name;
  }
};

//Ta bort ett recept från schemat
const removeRecipeFromSchedule = (day, meal) => {
  weeklySchedule.value[day][meal] = "";
};

//Ta bort recept från listan baserat på dess id
const removeRecipe = (id) => {
  recipes.value = recipes.value.filter((recipe) => recipe.id !== id);
};

//växla redigeringsläget
const doEdit = (e) => {
  editing.value = e;
  newRecipeName.value = "";
  newRecipeHighPriority.value = false;
};

//Ändra statusen
const toggleDone = (recipe) => {
  recipe.done = !recipe.done;
};

//Den uppdateras varje gång användaren ändrar storleken på webbläsarfönstret, tack vare eventlyssnaren för resize
const isMobile = ref(window.innerWidth <= 425);

window.addEventListener("resize", () => {
  isMobile.value = window.innerWidth <= 425;
});
</script>

<template>
  <div class="grid-container">
    <!-- Visar form där användaren kan mata in maträtt -->
    <header class="add-recipe-input">
      <h2>Veckans Måltidsplan</h2>
      <input class="input-recipe"
        v-model="newRecipeName"
        type="text"
        placeholder="Lägg till nytt maträtt"
      />
      <div class="add-recipe-choice">
        <label>
          <input type="checkbox" v-model="newRecipeHighPriority" />
          Favorit
        </label>
        <button
          @click="addNewRecipe"
          :disabled="newRecipeName.length < 3"
          class="btn btn-primary"
        >
          Lägg till
        </button>
      </div>
    </header>

    <!-- Visar veckans schema -->
    <main>
      <div class="table-wrapper">
        <table v-if="!isMobile">
          <thead>
            <tr>
              <th>Dag</th>
              <!-- För varje dag i veckan, och varje måltidstidpunkt skapas en cell i tabellen.-->
              <th v-for="meal in meals" :key="meal">{{ meal }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="day in daysOfWeek" :key="day">
              <td>{{ day }}</td>
              <td v-for="meal in meals" :key="meal">
                <!-- Visa recept om det finns -->
                <div v-if="weeklySchedule[day][meal]">
                  {{ weeklySchedule[day][meal] }}
                  <button
                    @click="removeRecipeFromSchedule(day, meal)"
                    class="btn btn-danger"
                  >
                    Ta bort
                  </button>
                </div>
                <!-- Visa dropdown om inget recept finns -->
                <div v-else>
                  <select
                    @change="
                      saveRecipeToSchedule(day, meal, $event.target.value)
                    "
                  >
                    <option disabled selected value="">Välj maträtt</option>
                    <option
                      v-for="recipe in recipes"
                      :key="recipe.id"
                      :value="recipe.id"
                      :class="{ priority: recipe.highPriority }"
                    >
                      {{ recipe.name }}
                    </option>
                  </select>
                </div>
              </td>
            </tr>
          </tbody>
        </table>

        <!-- Mobil vy: Visa som cards -->
        <div v-if="isMobile" class="mobile-schedule">
          <div v-for="day in daysOfWeek" :key="day" class="day-card">
            <h3>{{ day }}</h3>
            <div v-for="meal in meals" :key="meal" class="meal-entry">
              <strong>{{ meal }}:</strong>
              <div v-if="weeklySchedule[day][meal]">
                {{ weeklySchedule[day][meal] }}
                <button
                  @click="removeRecipeFromSchedule(day, meal)"
                  class="btn btn-danger"
                >
                  Ta bort
                </button>
              </div>
              <div v-else>
                <select
                  @change="saveRecipeToSchedule(day, meal, $event.target.value)"
                >
                  <option disabled selected value="">Välj maträtt</option>
                  <option
                    v-for="recipe in recipes"
                    :key="recipe.id"
                    :value="recipe.id"
                    :class="{ priority: recipe.highPriority }"
                  >
                    {{ recipe.name }}
                  </option>
                </select>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <aside>
      <h2>Tillgängliga recept</h2>
      <button v-if="editing" class="btn" @click="doEdit(false)">Avbryt</button>
      <button v-else class="btn btn-danger" @click="doEdit(true)">
        {{ changeButtonText }}
      </button>

      <!-- Visar alla tillgängliga recept -->
      <ul class="meal-list" v-if="editing" @submit.prevent="removeRecipe">
        <li
          v-for="(recipe, index) in recipes"
          @click="toggleDone(recipe)"
          :key="recipe.id"
          :class="{
            priority: recipe.highPriority,
          }"
        >
          {{ recipe.name }}
          <button @click="removeRecipe(recipe.id)" class="btn btn-danger">
            Ta bort
          </button>
        </li>
      </ul>
    </aside>
  </div>
</template>
