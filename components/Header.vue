<script setup lang="ts">

const menuList=ref()
const getMenuBar= async()=>{
  const {data: menu}:any=await useFetch('https://la3la3.com/shop-api/home/api/get-category.php')
  menuList.value=JSON.parse(menu.value)
}
const {data: menu , refresh}:any=await useFetch('https://la3la3.com/shop-api/home/api/get-category.php')
  menuList.value=JSON.parse(menu.value)

console.log(menuList.value)
const links = computed(()=>{
  return menuList.value.map((item:any)=>{
    return {
      label: item.name,
      to: `/products/${item.id}/catagory`,
      icon: 'i-heroicons-inbox',
    }
  })
})
watchEffect(()=>{
  getMenuBar()
})

</script>

<template>
  <UHeader :links="links">
    <template #logo>
      <img class="w-24 object-cover" src="https://clipart-library.com/images_k/computer-transparent-image/computer-transparent-image-5.png" alt="">
    </template>

    <template #right>
      <UColorModeButton />

      <UButton to="https://github.com/nuxt/ui" target="_blank" icon="cif:kh" color="gray" variant="ghost" />
    </template>

    <template #panel>
      <!-- <UNavigationTree :links="mapContentNavigation(navigation)" /> -->
    </template>
    <slot/>
  </UHeader>
</template>