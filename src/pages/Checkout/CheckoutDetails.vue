v
<template>
  <div class="q-my-xl">
    <div class="container">
      <div class="row items-center no-wrap justify-between">
        <h4 class="bigText q-pa-sm q-mb-md">Checkout</h4>
        <div class=""></div>
      </div>

      <h6>Shipping Address</h6>

      <!-- {{ route.name }} -->
      <div v-if="!authStore.token">
        <q-separator class="q-my-sm" />
        <p class="text-h6">
          Do you already have an account?
          <router-link
            :to="{
              name: 'customer.login',
              query: {
                redirect: route.name,
              },
            }"
          >
            Login
          </router-link>
        </p>
      </div>
      <div v-else>
        <q-separator class="q-my-sm" />
        <p>Fill out your delivery details to checkout</p>
      </div>
      <div class="cart">
        <div class="">
          <div class="small_container left">
            <div style="margin-top: 0" class="auth">
              <form @submit.prevent="handleCheckout">
                <div v-if="!authStore.token" class="input_wrap">
                  <label for="">Email Address <span>*</span></label>
                  <div class="input">
                    <input v-model="data.email" required type="email" />
                  </div>
                  <small v-if="errors.email" class="text-red text-weight-bold">
                    {{ errors.email[0] }}
                  </small>
                </div>
                <!-- <p class="text-info q-my-md smallerText">
                  You can create an account after checkout.
                </p> -->
                <q-separator />

                <div v-if="!authStore.token" class="input_wrap">
                  <label for="">First Name <span>*</span></label>
                  <div class="input">
                    <input v-model="data.firstname" required type="text" />
                  </div>

                  <small
                    v-if="errors.firstname"
                    class="text-red text-weight-bold"
                  >
                    {{ errors.firstname[0] }}
                  </small>
                </div>
                <div v-if="!authStore.token" class="input_wrap">
                  <label for="">Last Name <span>*</span></label>
                  <div class="input">
                    <input v-model="data.lastname" required type="text" />
                  </div>
                  <small
                    v-if="errors.lastname"
                    class="text-red text-weight-bold"
                  >
                    {{ errors.lastname[0] }}
                  </small>
                </div>
                <div v-if="!authStore.token" class="phone">
                  <label for="">Phone Number<span>*</span></label>
                  <div class="phone_wrap">
                    <div class="country_select">
                      <div class="input_wrap">
                        <div class="input">
                          <select v-model="country_code">
                            <option value="+234">+234 🇳🇬</option>
                          </select>
                        </div>
                      </div>
                    </div>
                    <div class="input_wrap">
                      <div class="input">
                        <input
                          v-model="data.phone"
                          placeholder="Enter phone number"
                          required
                          type="text"
                        />
                      </div>
                    </div>
                  </div>
                  <small v-if="errors.phone" class="text-red text-weight-bold">
                    {{ errors.phone[0] }}
                  </small>
                </div>
                <div v-if="!authStore.token" class="password">
                  <div class="input_wrap">
                    <label for=""> Password<span>*</span></label>
                    <div class="input row justify-between no-wrap">
                      <input
                        v-model="data.password"
                        required
                        :type="!viewConfirmPassword ? 'password' : 'text'"
                      />
                      <q-btn
                        @click="viewConfirmPassword = !viewConfirmPassword"
                        flat
                        icon="visibility"
                      >
                      </q-btn>
                    </div>
                    <small
                      v-if="errors.confirm_password"
                      class="text-red text-weight-bold"
                    >
                      {{ errors.confirm_password[0] }}
                    </small>
                    <small
                      v-if="errors.confirm_password"
                      class="text-red text-weight-bold"
                    >
                      {{ errors.confirm_password[0] }}
                    </small>
                  </div>
                </div>
                <div class="input_wrap">
                  <label for="">Street Address <span>*</span></label>
                  <div class="input">
                    <textarea
                      v-model="data.street_address"
                      required
                      cols="30"
                      rows="5"
                    ></textarea>
                  </div>
                </div>
                <!-- <div class="input_wrap">
                  <label for="">City <span>*</span></label>
                  <div class="input">
                    <input v-model="data.city" required type="text" />
                  </div>
                </div> -->
                <!-- <div class="input_wrap">
                <label for="">State/Province<span>*</span></label>
                <div class="input">
                  <select v-model="data.state" >
                    <option disabled value="">
                      Please, select a region, state or province
                    </option>
                  </select>
                </div>
              </div> -->

                <!-- <div class="input_wrap">
                <label for="">Country<span>*</span></label>
                <div class="input">
                  <select v-model="data.state" >
                    <option disabled value="">United States</option>
                  </select>
                </div>
              </div> -->
                <div class="input_wrap q-sel">
                  <label for=""> Country <span>*</span></label>
                  <div class="input">
                    <q-select
                      v-model="data.country_id"
                      use-input
                      @filter="filterFn"
                      behavior="dialog"
                      :options="countriesArr"
                      class="multi_select"
                      label="Select from the list of countiries"
                      style="width: 100%"
                      ><template v-slot:no-option>
                        <q-item>
                          <q-item-section class="text-grey">
                            No results
                          </q-item-section>
                        </q-item>
                      </template></q-select
                    >
                  </div>
                  <small
                    v-if="errors.country_id"
                    class="text-weight-bold text-red"
                  >
                    {{ errors.country_id[0] }}
                  </small>
                </div>

                <div class="input_wrap">
                  <label for=""
                    >State (select a country first) <span>*</span></label
                  >
                  <div class="input">
                    <!-- <select
                      :disabled="!statesList.length"
                      v-model="data.state_id"
                    >
                      <option disabled value="">Select State</option>
                      <option
                        v-for="(state, index) in statesList"
                        :key="index"
                        :value="state.id"
                      >
                        {{ state.name }}
                      </option>
                    </select> -->
                    <q-select
                      v-model="data.state_id"
                      :options="statesList"
                      label="Select State"
                      option-label="name"
                      option-value="id"
                      emit-value
                      map-options

                    />
                  </div>
                </div>
                <div class="input_wrap">
                  <label for="">City (select a state first)<span>*</span></label>
                  <div class="input">
                    <!-- <select :disabled="!cityList.length" v-model="data.city_id">
                      <option disabled value="">Select City</option>
                      <option
                        v-for="(city, index) in cityList"
                        :key="index"
                        :value="city.id"
                      >
                        {{ city.name }}
                      </option>
                    </select> -->
                    <q-select
                      v-model="data.city_id"
                      :options="cityList"
                      label="Select City"
                      option-label="name"
                      option-value="id"
                      emit-value
                      map-options

                    />
                  </div>
                </div>
                <div class="input_wrap">
                  <label for="">Zip/Postal Code <span>*</span></label>
                  <div class="input">
                    <input v-model="data.zip_code" required type="number" />
                  </div>
                </div>

                <q-separator class="q-mt-lg" />
                <div class="row justify-start q-mt-lg">
                  <q-btn
                    color="primary"
                    class="q-px-xl"
                    rounded
                    no-wrap
                    :loading="loadBtn"
                    no-caps
                    type="submit"
                  >
                    Next
                  </q-btn>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="checkout_details">
          <h4 class="bigMediumText">Order Summary</h4>
          <q-separator class="q-mt-md" />
          <p class="smallerText q-mb-md text-info">
            {{ cartStore.cart.length }} Items in Cart
          </p>
          <div class="q-my-lg">
            <div
              v-for="(item, index) in cartStore.cart"
              :key="index"
              style="gap: 0.5rem"
              class="row no-wrap q-my-md items-center"
            >
              <img
                style="width: 82px; height: 82px; object-fit: contain"
                :src="item.product.images[0].url"
                :alt="item.product.name"
              />
              <div>
                <p class="smallerText">
                  {{ item.product.name }}
                </p>
                <p>
                  <span class="text-info"
                    >Purchase quantity : {{ item.quantity }}</span
                  >
                </p>
                <p class="smallerText">
                  <!-- <span class="q-mx-sm"></span> -->
                  <span class="text-weight-bold"
                    >₦{{
                      (item.product.price * item.quantity).toLocaleString()
                    }}</span
                  >
                </p>
              </div>
              <q-separator />
            </div>
          </div>
          <q-separator class="q-my-md" />
          <div class="row q-mt-sm justify-between items-center">
            <p class="smallerText">VAT({{ vatRate }}%)</p>
            <p class="smallText">
              ₦{{ (cartStore.totalPrice * vatRate / 100).toLocaleString() }}
            </p>
          </div>
          <q-separator class="q-my-md" />
          <div class="row q-mt-sm justify-between items-center">
            <p class="smallerText">Sub Total</p>
            <p class="smallText">
              ₦{{ cartStore.totalPrice.toLocaleString() }}
            </p>
          </div>
          <q-separator class="q-my-md" />
          <div class="row q-mt-sm justify-between items-center">
            <p class="smallerText">Order Total</p>
            <p class="smallText">
              ₦{{
                (
                  cartStore.totalPrice +
                  cartStore.totalPrice * vatRate / 100
                ).toLocaleString()
              }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <FooterCompVue />
  <q-dialog class="dialog" v-model="orderSuccessModal" persistent>
    <div class="auth">
      <q-card class="billing">
        <div class="text-center q-mb-lg">
          <div class="bigMediumText">Continue to Payment!</div>
          <div class="smallText q-mt-md text-center">
            You have succesfully placed an order, proceed to payment...
          </div>
          <!-- <div class="smallText q-mt-md text-center">
            You have succesfully placed an order , now you can <br />
            keep track or your order till it reaches you
          </div> -->
        </div>

        <div class="total no-wrap column justify-center q-mt-md items-center">
          <!-- {{ paymentKind }} -->
          <div class="q-gutter-sm q-mb-md hidden">
            <q-radio v-model="paymentKind" val="DIRECT" label="Direct" />
            <q-radio v-model="paymentKind" val="ESCROW" label="Escrow" />
          </div>
          <q-btn
            class="apply bg-primary q-px-xl q-mt-md"
            no-caps
            flat
            @click="initPayment"
            rounded
            text-color="white"
          >
            Pay Now
          </q-btn>
          <!-- <q-btn
            class="apply bg-primary q-px-xl q-mt-md"
            no-caps
            flat
            :to="{ name: 'order.tracking' }"
            rounded
            text-color="white"
          >
            Track Order
          </q-btn> -->
        </div>
      </q-card>
    </div>
  </q-dialog>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import { useCartStore } from "src/stores/cart";
import FooterCompVue from "src/components/FooterComp.vue";
import { Loading, Notify, QSpinnerRings } from "quasar";
import { authAxios } from "src/boot/axios";
import { useMyAuthStore } from "src/stores/auth";
import { useRoute, useRouter } from "vue-router";
let route = useRoute();
let router = useRouter();
let cartStore = useCartStore();
let authStore = useMyAuthStore();
let data = ref({ city_id: "", state_id: "" });
let errors = ref({});
let loading = ref(false);
let loadBtn = ref(false);
let orderSuccessModal = ref(false);
let viewConfirmPassword = ref(false);
let countriesArr = ref([]);
let statesList = ref([]);
let cityList = ref([]);
let countriesBaseArr = [];
let country_code = ref("");
let paymentKind = ref("DIRECT");
let vatRate = ref();
watch(
  () => authStore.token,
  (newValue, oldValue) => {
    data.value = {};
  },
  { deep: true }
);

// watch(
//   () => data.value.country_id,
//   (newValue, oldValue) => {
//     if (newValue?.name !== oldValue?.name) {
//       Loading.show();
//       getState(newValue);
//       getCity(newValue);
//     }
//   },
//   { deep: true }
// );

watch(
  () => data.value.country_id,
  async (newCountry, oldCountry) => {
    // console.log("Country selected:", newCountry); // ✅ Debug log
    if (newCountry?.id !== oldCountry?.id) {
      data.value.state_id = ""; // Reset state
      data.value.city_id = ""; // Reset city
      // console.log("Fetching states for country:", newCountry.id); // ✅ Debug log
      await getState(newCountry.id);
    }
  },
  { deep: true }
);

watch(
  () => data.value.state_id,
  async (newState, oldState) => {
    // console.log("State select:", newState); // ✅ Debug log
    if (newState !== oldState) {
      data.value.city_id = ""; // Reset city when state changes
      // console.log("Fetching cities for state:", newState); // ✅ Debug log
      await getCity(data.value.country_id.id, newState);
    }
  },
  { deep: true }
);

const filterFn = (val, update, abort) => {
  if (val === "") {
    update(() => {
      countriesArr.value = countriesBaseArr;
    });
    return;
  }

  update(() => {
    const needle = val.toLowerCase();
    // console.log(val);
    countriesArr.value = countriesBaseArr.filter(
      (v) => v.name.toLowerCase().indexOf(needle) > -1
    );
  });
};
const getCountries = async () => {
  try {
    Loading.show();
    let countriesResp = await authAxios.get("country/supported-countries");
    countriesBaseArr = countriesResp.data.data
      .sort((a, b) => a.name.localeCompare(b.name))
      .map((country) => ({
        ...country,
        label: country.name,
        value: country.id,
      }));

    Loading.hide();
  } catch (error) {
    console.error(error);
  }
};
const onRequest = (props) => {
  if (authStore.token) {
    data.value = {
      ...authStore.userdetails,
      phone: authStore.userdetails.phone.substring(4),
    };
  }
};

// const getState = async (country) => {
//   try {
//     let statesListResp = await authAxios.get(
//       `country/states?country_id=${country.id}`
//     );

//     statesList.value = statesListResp.data.data;
//     Loading.hide();
//   } catch (error) {
//     console.error(error);
//     Loading.hide();
//   }
// };

const getState = async (countryId) => {
  // console.log("getState() called with country ID:", countryId); // ✅ Debug log
  try {
    if (!countryId) {
      console.error("Missing country ID for getState");
      return;
    }

    Loading.show();
    let response = await authAxios.get(`country/states?country_id=${countryId}`);
    statesList.value = response.data.data;
    // console.log("States received:", statesList.value); // ✅ Debug log
    Loading.hide();
  } catch (error) {
    console.error("Error fetching states:", error);
    Loading.hide();
  }
};

// const getCity = async (country) => {
//   try {
//     let cityListResp = await authAxios.get(
//       `country/cities?country_id=${country.id}`
//     );

//     cityList.value = cityListResp.data.data.sort((a, b) =>
//       a.name.localeCompare(b.name)
//     );
//     Loading.hide();
//   } catch (error) {
//     console.error(error);
//     Loading.hide();
//   }
// };

const getCity = async (countryId, stateId) => {
  // console.log("getCity() called with country ID:", countryId, "State ID:", stateId); // ✅ Debug log
  try {
    if (!countryId || !stateId) {
      console.error("Missing country or state ID for getCity");
      return;
    }

    Loading.show();
    let response = await authAxios.get(`country/cities?country_id=${countryId}&state_id=${stateId}`);
    cityList.value = response.data.data;
    // console.log("Cities received:", cityList.value); // ✅ Debug log
    Loading.hide();
  } catch (error) {
    console.error("Error fetching cities:", error);
    Loading.hide();
  }
};

const handleCheckout = () => {
  if (authStore.token) {
    updateShippingDetails();
  } else {
    loadBtn.value = true;
    authAxios
      .post("customer/signup", {
        firstname: data.value.firstname,
        lastname: data.value.lastname,
        email: data.value.email,
        phone: country_code.value + data.value.phone,
        password: data.value.password,
        confirm_password: data.value.password,
      })
      .then((response) => {
        console.log(response);
        // Notify.create({
        //   message: response.data.message,
        //   color: "green",
        //   position: "top",
        // });
        loadBtn.value = false;
        authStore.setUserDetails(response);
        updateShippingDetails();
      })
      .catch(({ response }) => {
        console.log(response);
        loadBtn.value = false;
        errors.value = response.data.data.errors || {};
        Notify.create({
          message: response.data.message
            ? response.data.message
            : "Recheck your credentials",
          color: "red",
          position: "top",
          actions: [{ icon: "close", color: "white" }],
        });
      });
  }
};
const updateShippingDetails = () => {
  Loading.show({
    spinner: QSpinnerRings,
    spinnerColor: "yellow",
    spinnerSize: 140,
    message: "Hold on... We are creating your order",
    messageColor: "white",
  });
  authAxios
    .post("customer/shipping/address/update", {
      // ...data.value,
      country_id: data.value.country_id.value,
      city_id: data.value.city_id,
      state_id: data.value.state_id,
      zip_code: data.value.zip_code.toString(),
      street_address: data.value.street_address,
    })
    .then((response) => {
      console.log(response);
      // loadBtn.value = false;
      authStore.userdetails.address = response.data.data.street_address;
      // authStore.userdetails.zip_code = response.data.data.zip_code;
      createOrder();
    })
    .catch(({ response }) => {
      errors.value = response.data.data.errors;
      Loading.hide();
      Notify.create({
        message: response.data.message,
        color: "red",
        position: "top",
        actions: [{ icon: "close", color: "white" }],
      });
    });
};
const createOrder = () => {
  // const cartItems = cartStore.cart.map((item, index) => ({
  //   [`cart_items[${index}][product_id]`]: item.product.id,
  //   [`cart_items[${index}][quantity]`]: item.quantity,
  // }));

  // Flatten the array to a single object
  // const dataToSend = Object.assign({}, ...cartItems);
  // console.log(dataToSend);
  if (!cartStore.cart.length) {
    Loading.hide();
    Notify.create({
      message: "You have not added any items to cart",
      color: "red",
      position: "top",
    });

    return;
  } else {
    const result = cartStore.cart.map((item) => ({
      product_id: item.product.id,
      quantity: item.quantity,
    }));
    const formData = new FormData();
    for (var key in result) {
      formData.append(key, JSON.stringify(result[key]));
    }
    authAxios
      .post(
        "customer/order/create",
        {
          cart_items: result,
        },
        {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        }
      )
      .then((response) => {
        console.log(response);
        loadBtn.value = false;
        Notify.create({
          message: response.data.message,
          color: "green",
          position: "top",
        });
        // cartStore.cart = [];

        cartStore.orderDetail = response.data.data;
        Loading.hide();
        orderSuccessModal.value = true;
      })
      .catch(({ response }) => {
        errors.value = response.data.data.errors;
        Notify.create({
          message: response.data.message,
          color: "red",
          position: "top",
          actions: [{ icon: "close", color: "white" }],
        });
        Loading.hide();
      });
  }
};
const initPayment = () => {
  if (paymentKind.value === "DIRECT") {
    Loading.show({
      spinner: QSpinnerRings,
      spinnerColor: "yellow",
      spinnerSize: 140,
      message: "Initializing payment...",
      messageColor: "white",
    });
    // customer/payment/initialize
    authAxios
      .post("customer/order/payment/initialize", {
        account_type: paymentKind.value,
      })
      .then((response) => {
        console.log(response);
        loadBtn.value = false;
        Notify.create({
          message: response.data.message,
          color: "green",
          position: "top",
        });
        cartStore.cart = [];

        // cartStore.orderDetail = response.data.data;
        console.log(response.data.data.paystack);
        Loading.hide();
        window.location.href = response.data.data.data.authorization_url;
        // router.replace({ name: "order.tracking" });
      })
      .catch(({ response }) => {
        Loading.hide();
        loadBtn.value = false;
        errors.value = response.data.data.errors;
        console.log(
          Object.values(response.data.data.errors).map((error) => error + ",")
        );
        Notify.create({
          message: response.data.message,
          color: "red",
          position: "top",
          actions: [{ icon: "close", color: "white" }],
        });
      });
  } else {
    Loading.show({
      spinner: QSpinnerRings,
      spinnerColor: "yellow",
      spinnerSize: 140,
      message: "Initializing payment...",
      messageColor: "white",
    });
    // customer/payment/initialize
    authAxios
      .post("customer/order/payment/initialize", {
        account_type: paymentKind.value,
      })
      .then((response) => {
        console.log(response);
        loadBtn.value = false;
        Notify.create({
          message: response.data.message,
          color: "green",
          position: "top",
        });
        cartStore.cart = [];

        // cartStore.orderDetail = response.data.data;
        console.log(response.data.data.paystack);
        Loading.hide();
        window.location.href = response.data.data.data.authorization_url;
        // router.replace({ name: "order.tracking" });
      })
      .catch(({ response }) => {
        Loading.hide();
        loadBtn.value = false;
        errors.value = response.data.data.errors;
        console.log(
          Object.values(response.data.data.errors).map((error) => error + ",")
        );
        Notify.create({
          message: response.data.message,
          color: "red",
          position: "top",
          actions: [{ icon: "close", color: "white" }],
        });
      });
  }
};

const fetchVAT = async () => {
  try {
    const response = await authAxios.get("/fees/show?code=VAT"); // Replace with actual endpoint
    vatRate.value = response.data.data.amount;
    // console.log(response.data.data.amount);
  } catch (error) {
    console.error("Error fetching VAT:", error);
  }
};

onMounted(async () => {
  try {
    onRequest();
    getCountries();
    fetchVAT();
  } catch (error) {
    console.error(error);
  }
});
</script>

<style lang="scss" scoped>
.cart {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1rem;
  .small_container.left {
    width: 90%;
  }
}

@media (max-width: 800px) {
  .cart {
    grid-template-columns: 1fr;
    .small_container.left {
      width: 100%;
    }
  }
}
</style>
