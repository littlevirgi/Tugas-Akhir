<template>
  <div>
    <div class="q-mb-md" v-if="form.payment_method == 'OVO'">
      <div class="bg-yellow-2 text-grey-7 q-pa-sm">
        <div class="text-weight-bold">Note:</div>
        <div>
          Anda memilih metode pembayaran OVO, <br>Pastikan nomor ponsel dan email yang anda inputkan sama dengan yang terdaftar di OVO milik anda.
        </div>
      </div>
    </div>
    <fieldset>
      <legend class="q-pa-sm">Info Pengiriman</legend>
        <table class="table dense">
          <tr>
            <th align="left">Penerima</th>
            <td>:</td>
            <td>{{ form.customer_name  }}</td>
          </tr>
          <tr>
            <th align="left">Whatsapp</th>
            <td>:</td>
            <td>{{ form.customer_whatsapp  }}</td>
          </tr>
          <tr>
            <th align="left">Alamat</th>
            <td>:</td>
            <td><div v-html="form.address"></div></td>
          </tr>
          <!--<tr>
            <th align="left">Kurir</th>
            <td>:</td>
            <td>{{ form.shipping_courier_name }}</td>
          </tr>
           <tr v-if="form.shipping_courier_service">
            <th align="left">Servis</th>
            <td>:</td>
            <td>{{ form.shipping_courier_service }}</td>
          </tr>-->
        </table>
    </fieldset>
    <fieldset class="q-mt-lg">
      <legend class="q-pa-sm">Ringkasan Order</legend>
        <div v-if="carts.length" class="bg-grey-2 q-mb-md">
          <q-list separator>
            <q-item v-for="cart in carts" :key="cart.id">
              <q-item-section side top>
              <q-img :src="cart.image_url" style="width:50px;height:50px;"></q-img>
              </q-item-section>
              <q-item-section>
                <div class="col">
                  <div class="text-weight-bold">{{ cart.name }}</div>
                  <div class="text-grey-7">{{ cart.quantity + 'x ' + moneyIDR(cart.price) }}</div>
                </div>
              </q-item-section>
            </q-item>
          </q-list>
        </div>
        <div class="flex justify-end">
          <table class="table dense">
            <!--<tr>
              <th align="right">Subtotal</th>
              <td align="right">:</td>
              <td align="right">{{ moneyIDR(form.subtotal) }}</td>
            </tr>
            <tr>
              <th align="right">Ongkos Kirim</th>
              <td align="right">:</td>
              <td align="right">{{ form.shipping_cost? moneyIDR(form.shipping_cost*0) : 0 }}</td>
            </tr>-->
            <tr>
              <th align="right">Total</th>
              <td align="right">:</td>
              <td align="right">{{ moneyIDR(form.subtotal) }}</td>
            </tr>
          </table>
        </div>
    </fieldset>
    <fieldset class="q-mt-lg" v-if="form.shipping_courier_name != 'COD'">
      <legend class="q-pa-sm">Pembayaran</legend>
        <div class="row q-gutter-sm">
          <template v-if="form.payment_method == 'COD'">
          <div class="box-shadow payment-list is-selected">
            <div class="text-weight-bold text-h5 text-center image">COD</div>   
            <div class="text-center name q-pa-xs">Bayar Ditempat</div>   
          </div>
          </template>
          <template v-else>
            <div class="box-shadow payment-list is-selected">
               <div class="image" v-if="payment.icon_url">
                <img  :src="payment.icon_url" />
              </div>
              <div class="flex justify-center items-center" v-if="payment.payment_type== 'DIRECT'" style="margin:auto;">
                <div class=" text-weight-bold text-md">{{  payment.bank_name }}</div>
                <div>Bayar ditempat</div>
              </div>
              <div v-else class="text-center name">
                {{ payment.name }}
              </div>
            </div>
          </template>
        </div>
    </fieldset>
  </div>
</template>

<script>
export default {
  name: 'ReviewOrder',
  props: {
    carts: Array,
    form: Object,
    payment: Object,
    noPayment: Boolean
  },
  methods: {
    money(number) {
     return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR'}).format(number)
    },
  }
}
</script>

<style>

</style>