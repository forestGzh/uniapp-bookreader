<template>
    <view>
        <view class="title">
            {{bookName}}
        </view>
        <view class="book_item" v-for="(chapter, index) in Chapters" :key="index">
            <navigator class="book_item_text" :url="'../chapter-content/chapter-content?chapterHref=' + chapter.chapterHref+'&chapterIndex=' + index">
                {{chapter.chapterName}}
            </navigator>
        </view>
    </view>
</template>

<script>
    const baseUrl = 'http://106.12.141.18:5000'
    const requestUrl = 'http://106.12.141.18:5000/chapters'
    const duration = 2000
    export default {
        data() {
            return {
                bookName: '',
                bookHref: '',
                Chapters: []
            };
        },
        onLoad: function(option) {
            console.log(option)
            this.bookName = option.bookName;
            this.bookHref = option.bookHref
            this.getChapters(option.bookHref)
        },
        methods: {
            // 获取章节列表
            getChapters(bookHref) {
                // http://106.12.141.18:5000/chapters?bookHref=%2Fbook%2F14733.html
                uni.request({
                    url: requestUrl,
                    dataType: 'json',
                    data: {
                        bookHref: bookHref
                    }
                }).then(res => {
                    console.log('request success', res[1]);
                    // this.Chapters = res[1].data.slice(0, 10);
                    this.Chapters = res[1].data;
                    uni.setStorage({
                        key: 'chapters',
                        data: this.Chapters,
                        success: function () {
                            console.log('success');
                        }
                    });
                }).catch(err => {
                    console.log('request fail', err);
                });
            },
        }
    }
</script>

<style>
    .title{
        text-align: center;
    }
    .book_item {
        text-align: left;
    }

    .book_item_text {
        height: 30rpx;
        padding: 20px;
        margin-top: 2px;
        /* background-color:#0A98D5; */
    }
</style>
