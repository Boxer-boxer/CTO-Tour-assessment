<template>
    <div>
        <div 
            class="image-container"
            :class="{'image-container-saved': selectedImages.indexOf(pic.image_id) >= 0}"
            @click="$emit('bookmark', pic.image_id)"
        >
            <img :src="pic.url" :alt="pic.title" class="image">
            <span 
                class="image-title"
                :class="{'image-title-saved': selectedImages.indexOf(pic.image_id) >= 0}"  
            >
                {{pic.title}}
            </span>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        pic: Object,
        selectedImages: Array
    }
}
</script>
<style lang="">
    
</style>