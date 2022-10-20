<template>
    <div class="tab-container">
      <button @click="$emit('fetch-All')">
        All
      </button>
        <button v-for="(value, key) in folders" :key="key" @click="$emit('filter-values', value)">
        {{key}}
      </button>
    </div>
</template>

<script>
export default {
    props: {
        folders: Object
    }
}
</script>
<style lang="">
    
</style>