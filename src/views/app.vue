<template>
    <div style="flex:1">
        <yunFile></yunFile>
    </div>
</template>

<!--引入bui-weex样式文件-->
<style lang="scss" src="bui-weex/src/css/buiweex.scss"></style>

<script>
    import yunFile from "./yunFile.vue";
    module.exports = {
        data () {
            return {}
        },
        components: {
          yunFile
        }
    }
</script>