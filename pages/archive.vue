<template>
  <div id="archive">
    <div class="grid grid-centered" style="max-width: 660px;padding: 0px 20px;margin-top: 80px">
      <div class="grid-cell" id="grid-cell">
        <div id="header_info">
          <!-- 顶部标题与分类区块 -->
          <nav class="header-nav reveal">
            <div class="cate-nav">
              <div>
                <nuxt-link to="/" class="top1 header-logo cate-name">{{ $t('lang.cate.archive') }}</nuxt-link>
              </div>
              <div>
                <nuxt-link to="/">
                  <b-button variant="primary" class="cate-back">
                    <i class="ri-arrow-left-line"></i>
                    {{ $t('lang.cate.backHome') }}
                  </b-button>
                </nuxt-link>
              </div>
            </div>
            <p class="top2 lead archive-p">{{ $t('lang.cate.archiveDes') }}</p>
          </nav>
          <topInsideCate :loading="false" />
          <!-- 顶部标题与分类区块 -->
        </div>

        <ul class="article-list">
          <!-- 加载骨架图 -->
          <li class="article-list-item reveal index-post-list bottom" v-if="loading">
            <div class="skeleton">
              <div class="skeleton-head"></div>
              <div class="skeleton-body">
                <div class="skeleton-title"></div>
                <div class="skeleton-content"></div>
              </div>
            </div>
          </li>
          <!-- 加载骨架图 -->

          <li
            class="article-list-item reveal index-post-list archive"
            v-for="(array,index_array) in posts_array"
            :key="'Year' + array.year"
          >
            <h2>{{ array.year }}</h2>
            <h3>{{ array.year }} / {{ parseInt(((array.posts[0].date.split('T'))[0].split('-'))[1]) }}</h3>

            <div v-for="(post,index) in array.posts" :key="'archivePost' + post.id">
              <div
                v-if="(index !== 0) && (parseInt(((post.date.split('T'))[0].split('-'))[1]) !== parseInt(((array.posts[index - 1].date.split('T'))[0].split('-'))[1]))"
              >
                <h3>{{ array.year }} / {{ parseInt(((post.date.split('T'))[0].split('-'))[1]) }}</h3>
              </div>
              <a class="post-a" :href="'/post/'+post.id">
                <p
                  v-html="'<em class=\'post-date\'>' + (post.date.split('T'))[0] + '</em>' + post.title.rendered"
                ></p>
              </a>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'

// import header-top-inside
import topInsideCate from '~/components/topInsideCate.vue'

@Component({
  components: {
    topInsideCate
  }
})
export default class Archive extends Vue {
  
  async asyncData(context: any): Promise<{ post_count: any; posts: any[] }> {
    let res1 = await Promise.all([
      // 获取博客文章数据
      context.$axios
        .get('https://blog.ouorz.com/wp-json/wp/v2/posts?per_page=1')
        .then((post_one: { data: any[] }): number => {
          return parseInt(post_one.data[0].post_total_count.post_count)
        })
    ])
    let res2 = await Promise.all([
      context.$axios
        .get('https://blog.ouorz.com/wp-json/wp/v2/posts?per_page=' + res1) //默认以发布时间排序
        .then((response: { data: any }) => {
          return response.data
        })
    ])
    return {
      post_count: res1,
      posts: res2[0]
    }
  }

  posts: any[] = []
  loading: boolean = true
  loading_des: boolean = false
  post_count: number = 0
  last_year: number = 0
  posts_array: any[] = []

  head() {
    return {
      title: 'TonyHe - 文章归档',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'TonyHe 的博客文章归档页'
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content:
            '贺莉朋,Tony,TonyHe,博客,个人博客,Personal Blog,blog,archive page,归档,归档页面,文章归档'
        }
      ]
    }
  }

  mounted() {
    var k = -1
    var i = 0
    let postsLength: number = this.posts.length
    for (i = 0; i < postsLength; i++) {
      //遍历所有文章
      if (
        this.articleDisplay(
          this.posts[i].post_categories[0].term_id,
          this.posts[i].post_metas.status,
          true
        )
      ) {
        if (this.posts[i].date.split('T')[0].split('-')[0] !== this.last_year) {
          //当前文章发布年与上一篇不同
          this.posts_array[(k += 1)] = [] //初始化数组
          this.posts_array[k]['posts'] = [] //初始化 posts 数组
          this.posts_array[k]['year'] = parseInt(
            this.posts[i].date.split('T')[0].split('-')[0]
          ) //增加年份
          this.posts_array[k]['posts'][
            this.posts_array[k]['posts'].length
          ] = this.posts[i] //增加文章
          this.last_year = this.posts[i].date.split('T')[0].split('-')[0] //赋值当前文章发布年份
        } else {
          //发布年份与上一篇相同
          this.posts_array[k]['posts'][
            this.posts_array[k]['posts'].length
          ] = this.posts[i] //增加文章
        }
      }
    }
    this.loading = false
  }

  articleDisplay = (cateId: number, status: any, type: boolean) => {
    if (type) {
      if (cateId !== 2 && cateId !== 58 && cateId !== 5 && !status) {
        return true
      } else {
        return false
      }
    } else {
      if (cateId !== 2 && cateId !== 58 && cateId !== 5 && !status) {
        return false
      } else {
        return true
      }
    }
  }
}
</script>