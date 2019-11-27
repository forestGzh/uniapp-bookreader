<template>
    <view>
        <rich-text :nodes="chapterContent"></rich-text>
        <view>
            <button @click="nextChapter">下一章</button>
        </view>
    </view>
</template>

<script>
    const baseUrl = 'http://106.12.141.18:5000'
    const requestUrl = 'http://106.12.141.18:5000/chapter'
    const duration = 2000
    export default {
        data() {
            return {
                chapters: [], //章节列表
                chapterHref: '', //章节链接
                chapterIndex: 0, //当前章节顺序
                chapterContent: '', //章节内容
            }
        },
        onLoad(option) {
            this.chapterHref = option.chapterHref;
            this.chapterIndex = option.chapterIndex;
            this.getChapterContent(option.chapterHref);
            this.getChapters();
        },
        onShow() {
            this.getChapters();
        },
        methods: {
            // 下一章
            nextChapter() {
                var index = parseInt(this.chapterIndex) + 1;
                console.log('index', index);
                var chapterHref = this.chapters[index].chapterHref;
                this.getChapterContent(chapterHref);
                this.chapterIndex = index;
                uni.pageScrollTo({
                    scrollTop: 0,
                    duration: 0
                });
            },
            // 章节列表
            getChapters() {
                uni.getStorage({
                    key: 'chapters',
                    success: (res) => {
                        this.chapters = res.data;
                    },
                    fail: () => {
                        console.log('fail');
                    }
                })
            },
            // 章节内容
            getChapterContent(chapterHref) {
                // http://106.12.141.18:5000/chapter?chapterHref=%2Fbook%2F14733%2F9471632.html
                uni.request({
                    url: requestUrl,
                    dataType: 'json',
                    data: {
                        chapterHref: chapterHref
                    }
                }).then(res => {
                    console.log('request success', res[1]);
                    this.chapterContent = res[1].data;
                    uni.setStorage({
                        key: 'chapterIndex',
                        data: this.chapterIndex,
                        success: function() {
                            console.log('success');
                        }
                    });
                }).catch(err => {
                    console.log('request fail', err);
                });
            },
            goTop: function(e) {
                this.scrollTop = this.old.scrollTop
                this.$nextTick(function() {
                    this.scrollTop = 0
                });
                uni.showToast({
                    icon: "none",
                    title: "纵向滚动 scrollTop 值已被修改为 0"
                })
            }
        }
    }
</script>

<style>

</style>
