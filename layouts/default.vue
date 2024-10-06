<template>
    <UDashboardLayout class="mt-20">
      <UDashboardPanel :width="265">
        
        <UDashboardSidebar >
           <MenuLeft />
          <div class="flex-1" />
          <UDivider class="sticky bottom-0" />
        </UDashboardSidebar>
      </UDashboardPanel>
  
      <slot />
    </UDashboardLayout>
  </template>