<script setup lang="ts">
import { ref, computed } from "vue";

function isWon() {
  return Math.floor(Math.random() * 2);
}

const krarkAmount = ref(0);
const flipAmount = ref(0);
const flipUntilMiss = ref(false);
const result = ref(0);

const testProba = async () => {
  const test = 100000;
  let amount = 0;
  for (let i = 0; i < test; i++) {
    amount += await flip(true);
  }
  alert(amount / test);
};

const flipUntilMissClass = computed(() => {
  return flipUntilMiss.value ? "green" : "";
});

const flip = async (test: boolean) => {
  let flips = 0;
  let flipsWon = 0;

  while (flips < flipAmount.value || flipUntilMiss.value) {
    let flipResult = 0;
    for (let i = 0; i < krarkAmount.value + 1; i++) {
      flipResult += isWon();
    }
    if (flipResult > 0) flipsWon++;
    if (flipResult === 0 && flipUntilMiss.value) break;
    flips++;
  }
  if (!test) {
    result.value = "...";
    await new Promise((r) => setTimeout(r, 500));
  }
  result.value = flipsWon;
  return flipsWon;
};
</script>

<template>
  <section>
    <div><h2>Krarkito</h2></div>
    <div><button v-on:click="testProba">Test Proba</button></div>
    <div>
      <button v-on:click="krarkAmount--">-</button>Krarks : {{ krarkAmount
      }}<button v-on:click="krarkAmount++">+</button>
    </div>
    <div>
      <button v-on:click="flipAmount--">-</button>Flips : {{ flipAmount
      }}<button v-on:click="flipAmount++">+</button>
    </div>
    <div>
      <button
        :class="flipUntilMissClass"
        v-on:click="flipUntilMiss = !flipUntilMiss"
      >
        Flip jusqu'à miss : {{ flipUntilMiss ? "YES" : "NO" }}
      </button>
    </div>
    <div>
      <button v-on:click="flip(false)">Flip</button>
    </div>
    <div>Résultat : {{ result }} réussis</div>
  </section>
</template>

<style>
#app {
  padding: 2rem 2rem;
  font-family: sans-serif;
}

section {
  width: 100vw;
  background-color: purple;
  color: white;
}

div {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: center;
  align-content: center;
  padding: 0 2rem;
  margin-bottom: 1rem;
}

h2 {
  display: block;
  margin-bottom: 2rem;
}

button {
  padding: 1rem;
}

.green {
  background-color: green;
}
</style>
