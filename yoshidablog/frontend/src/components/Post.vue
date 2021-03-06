<template>
    <article :key="id" class="container" v-if="post">
        <header>
            <nav id="back"><a @click="goBack" title="前ページへ戻る"><img src="@/assets/back.png"></a></nav>
            <p class="post-category" :style="{'color': post.category.color}">{{post.category.name}}</p>
            <h1 class="post-title">{{post.title}}</h1>
        </header>
        <div id="main">
            <nav id="toc" ref="toc"></nav>
            <div id="post-main" ref="text" v-html="post.main_text"></div>
        </div>
        <hr class="divider">
        <nav id="top"><a @click="scrollTop" title="一番上まで戻る"><img src="@/assets/ue.png"></a></nav>
    </article>
</template>

<script>
    export default {
        name: 'post',
        props: {
            id: {type: Number},
        },
        data() {
            return {
                post: null,
                hasBefore: false,
            }
        },
        beforeRouteEnter(to, from, next) {
            next(component => {
                if (from.name) {
                    component.hasBefore = true
                }
            })
        },
        mounted() {
            this.$http(`${this.$httpPosts}${this.id}/`, {credentials: "include",})
                .then(response => {
                    return response.json()
                })
                .then(data => {
                    this.post = data
                    document.title = `${data.title} - ブログ`
                    document.querySelector('meta[name="description"]').setAttribute('content', data.lead_text)
                    this.$nextTick(() => this.moveToc())
                })
        },
        methods: {
            goBack() {
                if (this.hasBefore) {
                    this.$router.go(-1)
                } else {
                    this.$router.push({name: 'posts'})
                }
            },
            scrollTop() {
                window.scrollTo({
                    top: 0,
                    behavior: "smooth"
                });
            },
            moveToc() {
                const innerToc = this.$refs.text.querySelector('div.toc')
                const cloneToc = innerToc.cloneNode(true)
                this.$refs.toc.appendChild(cloneToc)
            }
        }
    }
</script>

<style scoped>
    header {
        margin-bottom: 80px;
    }
    #back {
        margin-bottom: 80px;
    }
    #back a {
        cursor: pointer;
        width: 44px;
        display: inline-block;
    }
    #top a {
        cursor: pointer;
        color: #999;
        display: inline-block;
        width: 44px;
    }
    .post-category {
        font-size: 14px;
        font-family: fot-tsukuardgothic-std, sans-serif;
    }
    .post-title {
        font-weight: bold;
        font-size: 22px;
        font-family: fot-tsukuardgothic-std, sans-serif;
    }
    .divider {
        margin-top: 80px;
        margin-bottom: 80px;
        width: 100%;
        height: 1px;
        border: none;
        background-color: #ccc;
    }
    #post-main {
        width: 100%;
        line-height: 2;
    }
    #main >>> div.toc a {
        font-size: 12px;
        color: #666666;
        text-decoration: none;
    }
    #post-main >>> > * {
        margin-bottom: 1.5em;
    }
    #post-main >>> div.toc + * {
        margin-top: 0;
    }
    #post-main >>> > h2 {
        font-family: fot-tsukuardgothic-std, sans-serif;
        font-weight: bold;
        font-size: 18px;
        line-height: 1.5;
        margin-top: 42px;
        margin-bottom: 21px;
    }
    #post-main >>> img {
        max-width: 100%;
        height: auto;
        box-shadow: 0 0 5px #ccc;
    }
    #post-main >>> strong {
        background-color: yellow;
    }
    #post-main >>> div.toc {
        display: none;
    }
    #toc {
        display: none;
    }
    @media (min-width: 768px) {
        #post-main {
            width: 650px;
        }
    }
    @media (min-width: 1024px) {
        #main {
            display: grid;
            grid-template-columns: 150px 650px;
            column-gap: 50px
        }
        #toc {
            grid-column: 1;
            display: block;
            align-self: start;
            position: sticky;
            top: 20px;
        }
        #post-main {
            grid-column: 2;
        }
    }
</style>