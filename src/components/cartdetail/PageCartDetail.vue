<template>
  <div v-if="me"
       class="cart-page container">
    <div class="row">
      <div class="col-sm-8 col-xs-12">
        <div class="current-in-bag">
          <span>
            <img class="bag-icon-lg" src="../../assets/img/hand-bag-2-black.png" alt="bag icon">
          </span>
          <span class="text-uppercase your-bag-txt">{{ $t('yourBag') }}: </span>
          <span class="items-total-txt"
                data-test="cart-total-items">
            {{ $tc('itemsTotal', totalItems) }}
          </span>
        </div>
      </div>
    </div>
    <div v-if="cartNotEmpty">
      <div class="row">
        <div class="col-sm-12">
          <div class="cart-content">
            <CartLikeContentDetail :cartLike="me.activeCart"
                                   :editable="true"/>
            <AddDiscountCodeForm/>
            <CartLikePriceDetail :cartLike="me.activeCart"
                                 :editable="true"
                                 class="total-price-calc"/>
          </div>
        </div>
      </div>
      <div class="row bottom-cart-btns">
        <div class="col-sm-6">
          <router-link to="/"
                       class="text-uppercase continue-shopping-btn">
            {{ $t('continueShopping') }}
          </router-link>
        </div>
        <div class="col-sm-6">
          <div class="checkout-now checkout-now-bottom">
            <router-link :to="{ name: 'checkout'}"
                         class="pull-right text-uppercase checkout-now-btn">
              {{ $t('startCheckout') }}
            </router-link>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      {{ $t('empty') }}
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag';
import cartMixin from '../../mixins/cartMixin';
import CartLikeContentDetail from '../common/cartlike/CartLikeContentDetail.vue';
import CartLikePriceDetail from '../common/cartlike/CartLikePriceDetail.vue';
import AddDiscountCodeForm from './AddDiscountCodeForm.vue';
import CART_FRAGMENT from '../Cart.gql';
import ADDRESS_FRAGMENT from '../Address.gql';
import MONEY_FRAGMENT from '../Money.gql';

export default {
  components: {
    CartLikeContentDetail,
    CartLikePriceDetail,
    AddDiscountCodeForm,
  },

  data: () => ({
    me: null,
  }),

  mixins: [cartMixin],

  apollo: {
    me: {
      query: gql`
        query me($locale: Locale!) {
          me {
            activeCart {
              ...CartFields
            }
          }
        }
        ${CART_FRAGMENT}
        ${MONEY_FRAGMENT}
        ${ADDRESS_FRAGMENT}`,
      variables() {
        return {
          locale: this.$store.state.locale,
        };
      },
    },
  },
};
</script>

<i18n>
en:
  yourBag: "Your Bag"
  itemsTotal: "{n} item in total | {n} items in total"
  empty: "Your bag is empty"
  continueShopping: "Continue Shopping"
  startCheckout: "Start Checkout"
de:
  yourBag: "Ihr Einkaufswagen"
  itemsTotal: "{n} Artikel im Warenkorb"
  empty: "Ihr Einkaufswagen ist leer :("
  continueShopping: "Weiter einkaufen"
  startCheckout: "Zur Kasse"
</i18n>
