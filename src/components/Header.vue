<template>
    <header>
        <h1>{{title}}</h1>
        <Button v-show="homePage" @btn-click="$emit('toggle-add-task')" :text="btnText" :color="btnColor"></Button>
    </header>
</template>
<script>
    import Button from './Button.vue'
    
    export default {
    name: "Header",
    props: {
        title: {
            type: String,
            default: "HW"
        },
        btnColor: {
            type:String,
            default:'orange'
        },
        btnText: {
            type: String
        }
    },
    components: { Button },
    computed:{
        homePage(){
            if(this.$route.path === '/') {
                return true;
            }
            return false;
        }
    }
}
</script>
<style>
header {
    display : flex ;
    justify-content :space-between;
    align-items : center;
    margin-bottom : 20px;
}
</style>