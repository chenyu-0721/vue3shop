<script setup>
import { ref, computed } from 'vue';

const data = [
  { id: 1, name: "珍珠奶茶", description: "香濃奶茶搭配QQ珍珠", price: 50 },
  { id: 2, name: "冬瓜檸檬", description: "清新冬瓜配上新鮮檸檬", price: 45 },
  { id: 3, name: "翡翠檸檬", description: "綠茶與檸檬的完美結合", price: 55 },
  { id: 4, name: "四季春茶", description: "香醇四季春茶，回甘無比", price: 45 },
  { id: 5, name: "阿薩姆奶茶", description: "阿薩姆紅茶搭配香醇鮮奶", price: 50 },
  { id: 6, name: "檸檬冰茶", description: "檸檬與冰茶的清新組合", price: 45 },
  { id: 7, name: "芒果綠茶", description: "芒果與綠茶的獨特風味", price: 55 },
  { id: 8, name: "抹茶拿鐵", description: "抹茶與鮮奶的絕配", price: 60 },
];

const selectedOrders = ref([]);
const order = ref([]);
const orderPrice = ref(0);

const totalAmount = computed(() =>
  selectedOrders.value.reduce((total, order) => total + order.price * order.quantity, 0)
);

function sendButton() {
  order.value = selectedOrders.value;
  orderPrice.value = totalAmount.value;
  selectedOrders.value = [];
}

function addToOrder(item) {
  const existingOrder = selectedOrders.value.find(order => order.id === item.id);
  if (existingOrder) {
    existingOrder.quantity++;
  } else {
    selectedOrders.value.push({ ...item, quantity: 1 });
  }
}

function removeFromOrder(id) {
  selectedOrders.value = selectedOrders.value.filter(order => order.id !== id);
}
</script>

<template>
  <div id="root">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-4">
          <div class="list-group">
            <a
              v-for="item in data"
              :key="item.id"
              href="#"
              class="list-group-item list-group-item-action"
              @click.prevent="addToOrder(item)"
            >
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ item.name }}</h5>
                <small>${{ item.price }}</small>
              </div>
              <p class="mb-1">{{ item.description }}</p>
            </a>
          </div>
        </div>

        <div class="col-md-8">
          <table class="table" v-if="selectedOrders.length">
            <thead>
              <tr>
                <th scope="col" width="50">操作</th>
                <th scope="col">品項</th>
                <th scope="col">描述</th>
                <th scope="col" width="90">數量</th>
                <th scope="col">單價</th>
                <th scope="col">小計</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in selectedOrders" :key="order.id">
                <td><button type="button" class="btn btn-sm" @click="removeFromOrder(order.id)">x</button></td>
                <td>{{ order.name }}</td>
                <td><small>{{ order.description }}</small></td>
                <td>
                  <select class="form-select" v-model.number="order.quantity">
                    <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
                  </select>
                </td>
                <td>{{ order.price }}</td>
                <td>{{ order.price * order.quantity }}</td>
              </tr>
            </tbody>
          </table>
          <p class="d-flex justify-content-center align-item-center text-primary border pt-3 pb-3 bg-light" v-else>請選擇商品</p>
          <div class="text-end mb-3" v-if="selectedOrders.length">
            <h5>總計: <span>${{ totalAmount }}</span></h5>
          </div>
          <div class="text-end" v-if="selectedOrders.length">
            <button class="btn btn-primary" @click="sendButton()">送出</button>
          </div>
        </div>
      </div>
      <hr />
      <div class="row justify-content-center">
        <div class="col-8">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                <table class="table" v-if="order.length">
                  <thead>
                    <tr>
                      <th scope="col">品項</th>
                      <th scope="col">數量</th>
                      <th scope="col">小計</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="order in order" :key="order.id">
                      <td>{{ order.name }}</td>
                      <td>{{ order.quantity }}</td>
                      <td>{{ order.price * order.quantity }}</td>
                    </tr>
                  </tbody>
                </table>
                <p class="d-flex justify-content-center align-item-center text-primary border pt-3 pb-3 bg-light" v-else>尚未建立訂單</p>
                <div class="text-end" v-if="order.length">
                  <h5>總計: <span>${{ orderPrice }}</span></h5>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>