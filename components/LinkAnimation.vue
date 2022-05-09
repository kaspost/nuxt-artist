<template>
    <NuxtLink :to="link.slug">{{ link.title }}</NuxtLink> 
</template>


<script>
export default {
    props: ['link'],
}
</script>