<template>
    <view>
        <view class="book_item" v-for="(book, index) in books" :key="index">
            <navigator class="book_item_text" :url="'../booklist/booklist?bookHref=' + book.bookHref+'&bookName=' + book.bookName">
                {{book.bookName}}
            </navigator>
        </view>
        <button @click="sendRequest">发起请求</button>
    </view>
</template>

<script>
    const baseUrl = 'http://106.12.141.18:5000'
    const requestUrl = 'http://106.12.141.18:5000/books'
    const duration = 2000
    export default {
        data() {
            return {
                books: [{
                    "bookName": "1",
                    "bookHref": "/book/14733.html",
                    "chapters": null
                }]
            }
        },
        onLoad() {
            // this.sendRequest();  
        },
        methods: {
            sendRequest() {
                this._requestPromise();
            },
            _requestPromise() {
                uni.request({
                    url: requestUrl,
                    dataType: 'json',
                    data: {

                    }
                }).then(res => {
                    console.log('request success', res[1]);
                    this.books = res[1].data.slice(0,10);
                    console.log('data', this.books);
                }).catch(err => {
                    console.log('request fail', err);
                });
            },
        }
    }
</script>

<style>
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
