<script setup lang="ts">


const route = useRoute();
const productsList = ref<any>([]);
const sid = ref(0);
const mid = ref(0);
const s = ref(0);
const e = ref(12);
const loading = ref(false);

// Construct dynamic URL to avoid duplicate code
const getApiUrl = () => {
  const baseUrl = 'https://la3la3.com/shop-api/home/api/get-product.php';
  return sid.value === 0
    ? `${baseUrl}?cate-id=${mid.value}&s=${s.value}&e=${e.value}`
    : `${baseUrl}?sub-cate-id2=${sid.value}&s=${s.value}&e=${e.value}`;
};

const getProductApi = async () => {
  loading.value = true;
  try {
    const { data: products }: any = await useFetch(getApiUrl());
    productsList.value = JSON.parse(products.value);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  getProductApi();
});

watchEffect(() => {
  if (route.params.sid) {
    sid.value = Number(route.params.sid);
  }
  mid.value = Number(route.params.mid);
  getProductApi(); // Avoid resetting sid before the API call
});
const setImg=(img:any,item:any)=>{
  item.img=img
}
</script>
<template>
  <div >
    <div class="grid grid-cols-3 lg:grid-cols-4 gap-4 will-change-scroll p-4">
      <div
        class="h-[450px] bg-white dark:bg-neutral-800 rounded-b-lg mx-2 mb-6 lg:my-0 lg:mx-0 rounded-xl transform transition duration-300 hover:scale-95 hover:shadow-md hover:shadow-blue-500"
        v-for="item in productsList"
        :key="item.id"
      >
        <img :src="item.img" alt="" class="w-full h-[50%] object-center" />
        <div class="w-full h-[50%] p-3 ">
          <div class="w-full h-[35%] flex flex-col justify-center items-center">
            <p class="text-sm font-semibold text-center h-1/2">
              {{ item.name }}
            </p>
            <p
              class="text-sm font-semibold text-center mt-2 h-1/2  text-red-500"
            >
              {{ item.price }}$
            </p>
          </div>
          <div class="w-full h-[40%] flex justify-center items-center gap-2 p-2">
            <div
              v-for="img in item.imgList.slice(0, 4)"
              :key="img"
              class="w-[25%] h-full"
            >
              <img
                @click="setImg(img,item)"
                :src="img"
                alt=""
                :class="{ 'border-2 border-blue-500': item.img===img }"
                class="w-full h-full object-center block rounded-sm hover:border-blue-500 hover:border-2 cursor-pointer transition duration-300"
              />
            </div>
          </div>
          <button class="w-full h-[15%] bg-blue-500 text-white rounded-lg mt-4">Add To Cart</button>
        </div>
      </div>
    </div>
  </div>
</template>
