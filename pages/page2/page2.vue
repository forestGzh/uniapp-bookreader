<template>
    <view>
        <text> book</text>
        <view>
            {{book.bookName}}
        </view>
        <view>
            {{book.bookHref}}
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                bookName:'',
                bookHref:''
            };
        },
        onLoad: function(option) {
            this.bookName = option.bookname
        }
    }
</script>

<style lang="scss">

</style>
