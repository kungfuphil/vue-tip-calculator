<template>
  <h1>Tip Calculator</h1>
  <label>Total bill $</label>
  <input type="number" placeholder="Total Bill" v-model="totalBill" />

  <label>Tax Rate %</label>
  <input type="number" placeholder="Tax Rate" v-model="taxPercent" />
  <p>Subtotal (Before Tax) ${{ subTotal }}</p>

  <table>
    <thead>
      <tr>
        <th>Service Level</th>
        <th>Tip Percent</th>
        <th>Tip Total</th>
        <th>Grand Total</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Terrible</td>
        <td>10%</td>
        <td>${{ tip10 }}</td>
        <td>${{ grand10 }}</td>
      </tr>
      <tr>
        <td>Standard</td>
        <td>15%</td>
        <td>${{ tip15 }}</td>
        <td>${{ grand15 }}</td>
      </tr>
      <tr>
        <td>Good</td>
        <td>18%</td>
        <td>${{ tip18 }}</td>
        <td>${{ grand18 }}</td>
      </tr>
      <tr>
        <td>Excellent</td>
        <td>20%</td>
        <td>${{ tip20 }}</td>
        <td>${{ grand20 }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from "vue";

export default defineComponent({
  setup() {
    const taxPercent = ref<number>(7.5);
    const totalBill = ref<number>(0);

    // Computed values
    const taxRate = computed(() => {
      return taxPercent.value / 100;
    });

    const subTotal = computed(() => {
        let sub = totalBill.value / (1 + taxRate.value);
      return roundToTwo(sub);
    });

    const tip10 = computed(() => {
      return calculateTip(subTotal.value, 0.1);
    });
    const grand10 = computed(() => {
      return calculateGrandTotal(totalBill.value, tip10.value);
    });

    const tip15 = computed(() => {
      return calculateTip(subTotal.value, 0.15);
    });
    const grand15 = computed(() => {
      return calculateGrandTotal(totalBill.value, tip15.value);
    });

    const tip18 = computed(() => {
      return calculateTip(subTotal.value, 0.18);
    });
    const grand18 = computed(() => {
      return calculateGrandTotal(totalBill.value, tip18.value);
    });

    const tip20 = computed(() => {
      return calculateTip(subTotal.value, 0.20);
    });
    const grand20 = computed(() => {
      return calculateGrandTotal(totalBill.value, tip20.value);
    });

    // Functions
    const roundToTwo = (num: number) => {
      return +(Math.round(+(num + "e+2")) + "e-2");
    };
    const calculateTip = (subTotal: number, tipPercent: number) => {
      let tip = subTotal * tipPercent;
      return roundToTwo(tip);
    };
    const calculateGrandTotal = (totalBill: number, tip: number) => {
      let grandTotal = +totalBill + tip;
      return roundToTwo(grandTotal);
    };

    return {
      subTotal,
      totalBill,
      taxPercent,
      tip10,
      grand10,
      tip15,
      grand15,
      tip18,
      grand18,
      tip20,
      grand20,
    };
  },
});
</script>
