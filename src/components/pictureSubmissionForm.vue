<template>
    <form 
        class="message-form" 
        @submit.prevent="$emit('submit-picture', {currImage: currImage, folderName:folderName})"
    >
        <span class="validation-message">{{validationMessage}}</span>
        <label for="folder" class="message-bookmarked"> What folder should this image be saved for? </label>
        <input 
            type="text" 
            v-model="folderName" 
            class="message-input" 
            name='folder'
        />
        <input type="submit">
    </form>
</template>
<script>
export default {
    props: {
        validationMessage: String,
        currImage: Object
    },
    data() {
        return {
            folderName: ""
        }
    }
}
</script>
<style lang="">
    
</style>