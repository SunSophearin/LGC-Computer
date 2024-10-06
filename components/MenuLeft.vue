<script setup lang="ts">
import type { Base } from '#build/components';

const itemListSub = ref();
const mid = ref<Number>();
const route = useRoute();

const getMenuLeftApi = async () => {
  const { data: dataSub }: any = await useFetch(
    ` https://la3la3.com/shop-api/home/api/get-sub-category.php?cate-id=${mid.value}`
  );
  itemListSub.value = JSON.parse(dataSub.value);
};

const { data: dataSub }: any = await useFetch(
  ` https://la3la3.com/shop-api/home/api/get-sub-category.php?cate-id=${mid.value}`
);
itemListSub.value = JSON.parse(dataSub.value);

const linkss = computed(() => {
  return itemListSub.value.map((item: any) => {
    return {
      label: item.name,
      children: item.sub.map((sub: any) => {
        return {
          label: sub.name,
          to: `/products/${mid.value}/catagory/${sub.id}`,
        };
      }),
    };
  });
});
watchEffect(async () => {
  mid.value = Number(route.params.mid);
  await getMenuLeftApi();
});

onMounted(() => {
  getMenuLeftApi();
});
</script>
<template #panel>
  <div >
    <UNavigationTree
      :links="linkss"
      :multiple="false"
      :default-open="false"
    />
  </div>
</template>
