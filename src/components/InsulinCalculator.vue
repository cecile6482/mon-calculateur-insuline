<script setup>
import { ref } from 'vue';

const bloodSugar = ref(null);
const carbs = ref(null);
const insulinDosage = ref(null);

const calculateInsulin = () => {
  if (!bloodSugar.value || !carbs.value) {
    return;
  }

  const bloodSugarValue = parseFloat(bloodSugar.value);
  const carbsValue = parseFloat(carbs.value);

  const hour = new Date().getHours();
    let ratio;

    if (hour >= 6 && hour < 12) {
        ratio = 10 / 1.2;
        } else if (hour >= 12 && hour < 18) {
        ratio = 10 / 1.8;
        } else {
        ratio = 10 / 1.5;
    }


let insulinUnits = carbsValue / ratio;

if (bloodSugarValue < 0.8) {
    insulinUnits -= 1; 
} else if (bloodSugarValue > 1.5 && bloodSugarValue <= 2) {
    insulinUnits += 1;
} else if (bloodSugarValue > 2 && bloodSugarValue <= 2.5) {
    insulinUnits += 2;
} else if (bloodSugarValue > 2.5 && bloodSugarValue <= 3) {
    insulinUnits += 3;
} else if (bloodSugarValue > 3 && bloodSugarValue <= 3.5) {
    insulinUnits += 4;
} else if (bloodSugarValue > 3.5 && bloodSugarValue <= 4) {
    insulinUnits += 5;
}

insulinDosage.value = insulinUnits.toFixed(2);
};



</script>

<template>
  <div class="calculator">
    <h1>Insulin Calculator</h1>
    <p>Enter your blood sugar and carbohydrate intake to calculate your insulin dosage.</p>
    <form @submit.prevent="calculateInsulin" class="p-3">
        <div class="mb-3">
      <label for="blood-sugar" class="form-label">Blood Sugar</label>
      <input type="number" class="form-control" id="blood-sugar" name="blood-sugar" v-model="bloodSugar" step="0.01" />
        </div>  
        <div class="mb-3">
      <label for="carbs" class="form-label">Carbohydrates</label>
      <input type="number" class="form-control" id="carbs" name="carbs" v-model="carbs" step="0.01" />
        </div>
      <button type="submit" class="btn btn-primary">Calculate</button>
    </form>
    <p v-if="insulinDosage">Your insulin dosage is: {{ insulinDosage }}</p>
    
  </div>
</template>

<style scoped>

.calculator {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #c1bc38;
}


</style>