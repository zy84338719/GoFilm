<template>
  <div class="container" v-if="d.content.news.length > 0">
    <div class="title">
      <router-link :to="{ path: '/filmClassify', query: { Pid: d.title.id } }" class="h_active">{{ d.title.name }}</router-link>
      <span class="line"/>
      <router-link :to="{ path: '/filmClassifySearch', query: { Pid: d.title.id } }">{{ `${d.title.name}库` }}</router-link>
    </div>

    <!--影片列表展示-->
    <div class="content">
      <div class="news">
        <div class="c_nav">
          <span class="c_nav_text silver">最新上映</span>
          <router-link :to="{ path: '/filmClassifySearch', query: { Pid: d.title.id, Sort: 'release_stamp' } }" class="c_nav_more">更多<b class="iconfont icon-more"/></router-link>
        </div>
        <FilmList :col="7" :list="d.content.news"/>
      </div>
      <div class="news">
        <div class="c_nav">
          <span class="c_nav_text silver">排行榜</span>
          <router-link :to="{ path: '/filmClassifySearch', query: { Pid: d.title.id, Sort: 'hits' } }" class="c_nav_more">更多<b class="iconfont icon-more"/></router-link>
        </div>
        <FilmList :col="7" :list="d.content.top"/>
      </div>
      <div class="news">
        <div class="c_nav">
          <span class="c_nav_text silver">最近更新</span>
          <router-link :to="{ path: '/filmClassifySearch', query: { Pid: d.title.id, Sort: 'update_stamp' } }" class="c_nav_more">更多<b class="iconfont icon-more"/></router-link>
        </div>
        <FilmList :col="7" :list="d.content.recent"/>
      </div>
    </div>

    <!--分页展示区域-->
  </div>
</template>

<script setup lang="ts">
import { ApiGet } from "../../utils/request";
import { ElMessage } from "element-plus";
import { onMounted, reactive } from "vue";
import { useRouter } from "vue-router";
import FilmList from "../../components/index/FilmList.vue";
import { Bottom } from "@element-plus/icons-vue";

const d = reactive({
  title: {},
  content: {
    news: [],
    top: [],
    recent: [],
  }
});

const router = useRouter();

const getFilmData = () => {
  let query = router.currentRoute.value.query;
  ApiGet('/filmClassify', { Pid: query.Pid }).then((resp: any) => {
    if (resp.code === 0) {
      d.title = resp.data.title;
      d.content = resp.data.content;
    } else {
      ElMessage.error({ message: resp.msg, duration: 1000 });
    }
  });
};

onMounted(() => {
  getFilmData();
});
</script>


<style scoped>
@import "/src/assets/css/classify.css";

.c_nav {
  display: flex;
  justify-content: space-between;

  padding: 6px;
}

.c_nav_text {
  font-weight: 700;
  line-height: 1.1;
}

.c_nav_more {
  background: #25252b;
  border-radius: 8px;
}

.content > div {
  padding-bottom: 20px;
}

/**/
@media (min-width: 768px) {
  .c_nav{
    margin-bottom: 15px;
  }
  .c_nav_text {
    font-size: 28px;
  }
  .c_nav_more {
    font-size: 14px;
    padding: 0 15px;
    line-height: 32px;
  }

}

@media (max-width: 768px) {
  .c_nav{
    margin-bottom: 10px;
  }
  .c_nav_text {
    font-size: 20px;
    line-height: 28px;
  }
  .c_nav_more {
    font-size: 12px;
    padding: 0 10px;
    line-height: 28px;
  }
  .c_nav_more b{
    font-size: 12px;
  }
}

</style>