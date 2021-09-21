<template>
  <div id="app">
    <nav id="header" class="w-full top-10 py-1 bg-pink-300 shadow-lg">
      <div class="w-full flex items-center justify-between mt-0 px-6 py-2">
        <label for="menu-toggle" class="cursor-pointer md:hidden block">
          <svg class="fill-current text-blue-600" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20">
            <title>menu</title>
            <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"></path>
          </svg>
        </label>
        <input class="hidden" type="checkbox" id="menu-toggle" />

        <div class="hidden md:flex md:items-center md:w-auto w-full order-3 md:order-1" id="menu">
          <nav>
            <ul class="md:flex items-center justify-between text-base text-blue-600 pt-4 md:pt-0">
              <li>
                <a class="inline-block no-underline hover:text-black font-medium text-lg py-2 px-4 lg:-ml-2" href="#">Home</a>
              </li>
              <li>
                <a class="inline-block no-underline hover:text-black font-medium text-lg py-2 px-4 lg:-ml-2" href="#">Products</a>
              </li>
              <li>
                <a class="inline-block no-underline hover:text-black font-medium text-lg py-2 px-4 lg:-ml-2" href="#">About</a>
              </li>
            </ul>
          </nav>
        </div>

        <div class="order-2 md:order-3 flex flex-wrap items-center justify-end mr-0 md:mr-4" id="nav-content">
          <div class="auth flex items-center w-full md:w-full">
            <button class="bg-transparent text-gray-800 p-2 rounded border border-gray-300 mr-4 hover:bg-gray-100 hover:text-gray-700">Sign in</button>
            <button class="bg-blue-600 text-gray-200 p-2 rounded hover:bg-blue-500 hover:text-gray-100">Sign up</button>
          </div>
        </div>
      </div>
    </nav>
    <div>
      <!-- Container -->
      <div class="">
        <div class="p-12 bg-grey h-screen">
          <!-- Row -->
          <div class="w-full flex">
            <!-- Col -->
            <div class="w-full h-auto p-6 border border-blue-50 lg:w-5/12 bg-cover rounded-l-lg bg-white rounded shadow-lg">
              <h3 class="pt-4 text-2xl text-center text-blue-600">Buat Pesanan</h3>
              <form v-on:submit.prevent="addNewProduct" class="p-4">
                <div class="grid grid-cols-4 gap-4 p-2">
                  <div class="md:col-span-1 col-span-4 text-blue-600">Kode Barang</div>
                  <div class="md:col-span-3 col-span-4">
                    <input type="text" v-model="inputP.code" class="border border-pink-300 rounded" />
                  </div>
                </div>
                <div class="grid grid-cols-4 gap-4 p-2">
                  <div class="md:col-span-1 col-span-4 font text-blue-600">Nama Barang</div>
                  <div class="md:col-span-3 col-span-4">
                    <input type="text" v-model="inputP.name" class="border border-pink-300 rounded" />
                  </div>
                </div>
                <div class="grid grid-cols-4 gap-4 p-2">
                  <div class="md:col-span-1 col-span-4 text-blue-600">Tipe Barang</div>
                  <div class="md:col-span-3 col-span-4">
                    <select v-model="inputP.typeID" class="border border-pink-300 rounded" id="">
                      <option v-for="item in items" :value="item.id" :key="item.id">
                        {{ item.type }}
                      </option>
                    </select>
                  </div>
                </div>
                <div class="grid grid-cols-4 gap-4 p-2">
                  <div class="md:col-span-1 col-span-4 text-blue-600">Harga Barang</div>
                  <div class="md:col-span-3 col-span-4">
                    <input type="number" v-model="inputP.price" class="border border-pink-300 rounded" />
                  </div>
                </div>
                <div class="grid grid-cols-4 gap-4 p-2">
                  <button class="py-3 px-6 text-blue-600 rounded-lg bg-pink-300 shadow-lg block md:inline-block">Save</button>
                </div>
                <div class="grid grid-cols-4 gap-4 p-2">
                  {{ message }}
                </div>
              </form>
            </div>
            <!-- Col -->
            <div class="ml-8 border-white shadow-lg p-2">
              <h3 class="pt-3 pb-4 text-2xl text-center text-blue-600">Product List</h3>
              <table class="">
                <thead>
                  <tr class="text-md font-semibold tracking-wide text-center text-blue-600 uppercase border-b">
                    <th class="bg-white text-center p-3">Code</th>
                    <th class="bg-white text-center p-3">Name</th>
                    <th class="bg-white text-center p-3">Type</th>
                    <th class="bg-white text-center p-3">Price</th>
                    <th class="bg-white text-center p-3 w-44">Quantity</th>
                    <th class="bg-white text-center p-3">Total Harga</th>
                    <th class="bg-white text-center p-3">Aksi</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, index) in products" :key="index" class="text-gray-700">
                    <td class="border text-center">
                      {{ item.code }}
                    </td>
                    <td class="border text-center">
                      {{ item.name }}
                    </td>
                    <td class="border text-center">
                      {{ getType(item.typeID) }}
                    </td>
                    <td class="border text-center">
                      {{ item.price }}
                    </td>
                    <td class="border flex justify-center items-center">
                      <button class="px-2 py-1 rounded-full bg-black text-white" @click="handleMinButton(item.code)" type="button">-</button>
                      <span class="mx-2">
                        {{ item.quantity }}
                      </span>
                      <button class="px-2 py-1 rounded-full bg-pink-300 text-white hover:bg-pink-400" @click="handlePlusButton(item.code)" type="button">+</button>
                    </td>
                    <td class="border text-center">
                      {{ totalHarga(item.code) }}
                    </td>
                    <td class="border text-center">
                      <button class="py-1 px-2 text-white bg-black rounded-full" v-on:click="products.splice(index, 1)">Remove</button>
                      <button class="py-1 px-6 text-white bg-pink-300 rounded-full" v-on:click="editProduct(index, item)">Edit</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      inputP: {
        quantity: 1,
      },
      message: "",
      items: [
        {
          id: 1,
          type: "Minuman",
        },
        {
          id: 2,
          type: "Makanan",
        },
      ],
      products: [],
      productID: 1,
      ind: null,
    };
  },
  methods: {
    addNewProduct: function () {
      if (this.ind != null) {
        Object.assign(this.products[this.ind], {
          code: this.inputP.code,
          name: this.inputP.name,
          typeID: this.inputP.typeID,
          price: this.inputP.price,
          quantity: this.inputP.quantity,
        });
      } else {
        this.products.push({
          code: this.inputP.code,
          name: this.inputP.name,
          typeID: this.inputP.typeID,
          price: this.inputP.price,
          quantity: this.inputP.quantity,
        });
      }
      this.inputP.code = "";
      this.inputP.name = "";
      this.inputP.typeID = "";
      this.inputP.price = "";
      this.ind = null;
      this.inputP.quantity = 1;
    },

    getItem: function (id) {
      let res = this.products.filter((e) => {
        if (e.code == id) return e;
      });
      return res;
    },

    editProduct: function (ind, p) {
      this.ind = ind;
      this.inputP.code = p.code;
      this.inputP.name = p.name;
      this.inputP.typeID = p.typeID;
      this.inputP.price = p.price;
      this.inputP.quantity = p.quantity;
    },

    handlePlusButton: function (id) {
      this.getItem(id)[0].quantity++;
    },

    handleMinButton: function (id) {
      if (this.getItem(id)[0].quantity != 1) this.getItem(id)[0].quantity--;
    },

    getType: function (id) {
      var m = this.items.filter(function (elem) {
        if (elem.id == id) return elem;
      });
      return m[0].type;
    },
    totalHarga: function (id) {
      let res = this.getItem(id);
      return res[0].quantity * res[0].price;
    },
  },
};
</script>
