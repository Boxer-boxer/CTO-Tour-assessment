<template>
    <div>
        <div 
            class="image-container"
            :class="{'image-container-saved': selectedImages.indexOf(pic.image_id) >= 0}"
            @click="$emit('bookmark', pic.image_id)"
        >
            <img :src="pic.url" :alt="pic.title" class="image">
            <div class="image-title-display">
                <span 
                    class="image-title"
                    :class="{'image-title-saved': selectedImages.indexOf(pic.image_id) >= 0}"  
                >
                    {{pic.title}}
                </span>
                <img src="../assets/arrow.png" class="arrow-right" alt="Arrow Right">
            </div>
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
<style>
    .image-title-display {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .arrow-right {
        width: 25px;
    }
</style>