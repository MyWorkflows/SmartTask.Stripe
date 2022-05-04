<script lang="ts">
import { defineComponent } from "vue";
import { StripeCheckout } from "@vue-stripe/vue-stripe";

export interface IStateModel {
  priceId: string;
  price: number;
  currency: string;
  productName: string;
  productDescription: string;
}

export default /*#__PURE__*/ defineComponent({
  name: "SmartTaskComponent", // vue component name
  props: ["componentState", "componentStatus"],
  components: {
    StripeCheckout,
  },
  data() {
    return {
      model: {
        priceId: "price_1KvZy2LmsBhsnW8s4fC3qwGP",
        price: 20,
        currency: "SGD",
        productName: "Test Product",
        productDescription: "Test Product Description",
      } as IStateModel,
      publishableKey:
        "pk_test_51KvZrULmsBhsnW8smgaE7G0xPcUTqGKLeSdNAwdNcVsFUcy56Eqec96kqHLdshFoCWzFq2r0ycX82cyRT684EVQv002mMCy5ZN",
      loading: false,
      lineItems: [
        {
          price: "price_1KvZy2LmsBhsnW8s4fC3qwGP", // The id of the one-time price you created in your Stripe dashboard
          quantity: 1,
        },
      ],
      successURL: "https://myworkflows.io",
      cancelURL: "https://myworkflows.io",
    };
  },
  methods: {
    submit() {
      // You will be redirected to Stripe's secure checkout page
      (this.$refs.checkoutRef as any).redirectToCheckout();
    },
  },
});
</script>

<template>
  <div>
    <stripe-checkout
      ref="checkoutRef"
      mode="payment"
      :pk="publishableKey"
      :line-items="lineItems"
      :success-url="successURL"
      :cancel-url="cancelURL"
      @loading="(v) => (loading = v)"
    />
    <div class="card-plan">
      <div class="card-plan-img">
        <img
        height="90"
          src="https://cdn2.iconfinder.com/data/icons/e-commerce-line-4-1/1024/open_box4-512.png"
          alt=""
        />
      </div>
      <div class="card-plan-text">
        <div class="card-plan-title">{{ model.productName }}</div>
        <p>{{ model.productDescription }}</p>
        <div class="card-plan-price">
          {{ model.price }} {{ model.currency }}
        </div>
      </div>
    </div>
    <div class="card-payment-button">
      <button class="btn btn-info" @click="submit">Proceed to Payment</button>
    </div>
  </div>
</template>

<style scoped>
.card .card-body .card-payment-button {
  padding: 25px 0px 15px;
  box-sizing: border-box;
}
.card .card-body .card-payment-button button {
  width: 100%;
  height: 50px;
  border: 0;
  background: hsl(245, 75%, 52%);
  color: white;
  font-weight: 700;
  border-radius: 10px;
  box-shadow: 0px 10px 20px 0px hsl(245deg 75% 52% / 44%);
  cursor: pointer;
}
.card .card-body .card-payment-button button:hover {
  background: #766cf1;
}
.card .card-body .card-title {
  width: 100%;
  font-weight: 900;
  color: hsl(223, 47%, 23%);
  text-align: center;
  padding: 15px;
  box-sizing: border-box;
}
.card .card-body .card-text {
  width: 100%;
  color:hsl(224, 23%, 55%);
  text-align: center;
  line-height: 25px;
  padding: 15px 0px;
  box-sizing: border-box;
}
.card .card-body .card-plan {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 15px;
  background: hsl(0, 0%, 100%);
  border-radius: 10px;
  padding: 15px;
  box-sizing: border-box;
}
.card .card-body .card-plan .card-plan-img {
  flex-grow: 1;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.card .card-body .card-plan .card-plan-text {
  flex-grow: 6;
  display: flex;
  flex-direction: column;
  row-gap: 4px;
}
.card .card-body .card-plan .card-plan-text .card-plan-title {
  color:hsl(223, 47%, 23%);
  font-weight: 900;
  font-size: 14px;
}
.card .card-body .card-plan .card-plan-text .card-plan-price {
  color: hsl(224, 23%, 55%);
  font-size: 14px;
}
</style>
