<script lang="ts">
import { loadStripe } from "@stripe/stripe-js";
import { defineComponent, ref, onBeforeMount } from "vue";
// @ts-ignore
import { VuePaycard } from "vue-paycard";

export default /*#__PURE__*/ defineComponent({
  name: "SmartTaskComponent", // vue component name
  props: ["componentState", "componentStatus"],
  components: {
    VuePaycard,
  },
  data() {
    return {
      model: {
        cardName: "",
        cardNumber: "",
        cardMonth: "",
        cardYear: "",
        cardCvv: "",
      },
    };
  },
  computed: {},
  methods: {
    async submitForm() {
      this.$emit("execute-smart-task", this.model);
    },
    pay() {
      // Get stripe element
      const cardElement = this.card.stripeElement;

      // Access instance methods, e.g. createToken()
      this.elms.instance.createToken(cardElement).then((result: object) => {
        // Handle result.error or result.token
        console.log(result);
      });
    },
  },
  created() {
    if (this.componentState) {
      this.model = this.componentState;
    }
  },
  setup() {
    const stripeKey = ref("pk_test_TYooMQauvdEDq54NiTphI7jx"); // test key
    const instanceOptions = ref({
      // https://stripe.com/docs/js/initializing#init_stripe_js-options
    });
    const elementsOptions = ref({
      // https://stripe.com/docs/js/elements_object/create#stripe_elements-options
    });
    const cardOptions = ref({
      // https://stripe.com/docs/stripe.js#element-options
      value: {
        postalCode: "12345",
      },
    });
    const stripeLoaded = ref(false);
    const card = ref();
    const elms = ref();

    onBeforeMount(() => {
      const stripePromise = loadStripe(stripeKey.value);
      stripePromise.then(() => {
        stripeLoaded.value = true;
      });
    });

    return {
      stripeKey,
      stripeLoaded,
      instanceOptions,
      elementsOptions,
      cardOptions,
      card,
      elms,
    };
  },
});
</script>

<template>
  <div class="row">
    <div class="col-6">
      <VuePaycard :value-fields="model" />
    </div>
    <div class="col-6">
      <div class="row  mb-2">
        <div class="col-sm-12">
          <div class="form-group">
            <label for="name">Name</label>
            <input
              v-model="model.cardName"
              class="form-control"
              id="name"
              type="text"
              placeholder="Enter your name"
            />
          </div>
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-sm-12">
          <div class="form-group">
            <label for="ccnumber">Credit Card Number</label>
            <div class="input-group">
              <input
                v-model="model.cardNumber"
                class="form-control"
                type="text"
                placeholder="0000 0000 0000 0000"
                autocomplete="cc-number"
              />
              <div class="input-group-append">
                <span class="input-group-text">
                  <i class="bi bi-credit-card"></i>
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row  mb-2">
        <div class="form-group col-sm-4">
          <label for="ccmonth">Month</label>
          <select v-model="model.cardMonth" class="form-control" id="ccmonth">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
            <option>6</option>
            <option>7</option>
            <option>8</option>
            <option>9</option>
            <option>10</option>
            <option>11</option>
            <option>12</option>
          </select>
        </div>
        <div class="form-group col-sm-4">
          <label for="ccyear">Year</label>
          <select v-model="model.cardYear" class="form-control" id="ccyear">
            <option>2014</option>
            <option>2015</option>
            <option>2016</option>
            <option>2017</option>
            <option>2018</option>
            <option>2019</option>
            <option>2020</option>
            <option>2021</option>
            <option>2022</option>
            <option>2023</option>
            <option>2024</option>
            <option>2025</option>
          </select>
        </div>
        <div class="col-sm-4">
          <div class="form-group">
            <label for="cvv">CVV/CVC</label>
            <input
              class="form-control"
              id="cvv"
              type="text"
              placeholder="123"
            />
          </div>
        </div>
      </div>
      <div class="row mx-1">
        <button class="btn btn-primary">Pay</button>
      </div>
    </div>
  </div>
</template>
