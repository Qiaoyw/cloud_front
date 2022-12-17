<template>
    <div>
<!--      导航-->
      <v-tabs
          fixed-tabs
          background-color="indigo"
          dark
          style="width: 90%; margin-left: 5%"
          v-model="value"
      >
        <v-tab>
          预防知识
        </v-tab>
        <v-tab
            @click="toQuestionList()">
          问题列表
        </v-tab>
      </v-tabs>

<!--    提问模块-->
    <v-dialog
        transition="dialog-bottom-transition"
        max-width="600"
        v-model="dialog"
      >
        <v-card>
            <v-toolbar
              color="primary"
              dark
            >在这里填写问题</v-toolbar>
            <v-card-text>
              <div>
                <br>
                <v-text-field
                label="问题标题"
                :rules="rules"
                hide-details="auto"
                v-model="title"
                >
                  {{title}}
                </v-text-field>
                <br>
                <v-text-field label="描述一下您的问题"
                v-model="content">
                  {{content}}
                </v-text-field>
            </div>
            </v-card-text>
            <v-card-actions class="justify-end">
            <v-btn
                text
                color="pink"
                @click="postQuestion(title, content)"
              >提交</v-btn>
              <v-btn
                text
                @click="dialog= false"
              >关闭</v-btn>
            </v-card-actions>
          </v-card>
      </v-dialog>

<!--      疫情知识模块-->
      <v-container
          class="d-flex justify-center">
        <v-row
            style="margin-top: 10px">
          <v-col
            style="background-color: white"
          md="10"
          offset-md="1">
<!--            <p class="text-h4 font-weight-bold">-->
<!--              <v-icon-->
<!--                  large-->
<!--                  color="blue darken-2"-->
<!--              >-->
<!--                mdi-message-text-->
<!--              </v-icon>-->
<!--              预防常识</p>-->
            <v-simple-table
            style="border-radius: 0px;"
            >
              <template
                  v-slot:default
              >
                <tbody>
                <tr
                    v-for="(item,index) in knowledge"
                    :key="index"

                >
                  <td
                      style="height: 120px;cursor: pointer;"
                      @click="toLocale(item.url)"
                  >
                    <p class="text-h4 font-weight-bold">
                      <v-icon
                          large
                          color="blue darken-2"
                      >
                        mdi-message-text
                      </v-icon>
                      {{ item.title}}
                    </p>
                    <p class="font-weight-regular" style="margin-left: 15px">{{item.date}}</p>
                  </td>
                </tr>
                </tbody>
              </template>
            </v-simple-table>
          </v-col>
        </v-row>
      </v-container>
<!--        <v-row style="margin-top:35px;margin-right:40px;margin-left: 40px">-->
<!--                <v-card-->
<!--                    class="mx-auto"-->
<!--                    style="margin:20px 0px 30px 0px;"-->
<!--                    color="#B0BEC5"-->
<!--                    dark-->
<!--                    v-for="(tip, index) in tips.slice(0,16)"-->
<!--                    :key="index"-->
<!--                    width="240px"-->
<!--                    height="240px"-->
<!--                >-->
<!--                  <v-card-title>-->
<!--&lt;!&ndash;                    <v-icon&ndash;&gt;-->
<!--&lt;!&ndash;                        middle&ndash;&gt;-->
<!--&lt;!&ndash;                        left&ndash;&gt;-->
<!--&lt;!&ndash;                        style="float: left"&ndash;&gt;-->
<!--&lt;!&ndash;                    >&ndash;&gt;-->
<!--&lt;!&ndash;                      mdi-twitter&ndash;&gt;-->
<!--&lt;!&ndash;                    </v-icon>&ndash;&gt;-->
<!--                    <h3-->
<!--                        class="text-h7  align-center"-->
<!--                        style="font-weight: 700; height: 20px;margin-top:-5px;margin-bottom: 8px;margin-left: 4px"-->
<!--                        >-->
<!--                      {{tip.title}}</h3>-->
<!--                  </v-card-title>-->
<!--                  <v-divider></v-divider>-->
<!--                <v-card-text>-->
<!--                  <div class="text&#45;&#45;primary" style="height: 100px">-->
<!--                      {{tip.content.slice(0,45)+"..."}}-->
<!--                  </div>-->
<!--                </v-card-text>-->
<!--                  <v-btn-->
<!--                      :disabled="loading"-->
<!--                      class="ma-1"-->
<!--                      color="white"-->
<!--                      plain-->
<!--                      style=""-->
<!--                      @click="toSheet(tip.id)"-->
<!--                  >-->
<!--                    查看更多...-->
<!--                  </v-btn>-->
<!--                </v-card>-->
<!--        </v-row>-->
    </div>


</template>

<script>
export default {
    data(){
        return{
            value:0,
            dialog: false,
            tips:[],
            title:"",
            content:"",
            knowledge:[
              {
                title:"新冠病毒感染者就医指南",
                date:"近期新冠肺炎流行，对群众正常生活带来了不小的影响，为倡导全市共同行动起来，响应支持国家“新十条”优化政策，科学防控，有序就医，规范治疗，最大限度保障群众生命安全和身体健康。",
                url:"https://new.qq.com/rain/a/20221215A04Z5700"
              },
              {
                title:"出现阳性症状怎么办?",
                date:"在目前激增的就医人员中，绝大多数没必要到发热门诊去扎堆。有医疗专家呼吁，如果只是轻微不适就不要给自己增大感染风险，希望大家非必要不来医院。",
                url:"https://baijiahao.baidu.com/s?id=1752423498228375744&wfr=spider&for=pc"
              },
              {
                title:"面对新冠，决不慌乱！",
                date:"北京大学为了让老师和同学们更好地理解当前的疫情防控政策，以科学理性的态度认识新冠疫情，以积极向上的心态进行自我防护。2022年12月8日，学校邀请国务院联防联控机制医疗救治专家组成员、北京大学第一医院感染疾病科主任王贵强教授，为全校师生开展“科学应对新冠疫情”专题讲座。" ,
                url:"https://mp.weixin.qq.com/s/jMvFsKX7XI_lF05-OKDP1g"
              },
              {
                title:"“我阳了，怎么办？”北大医学专家贴心支招！",
                date:"国务院联防联控机制组织权威专家，中国工程院院士、北京大学常务副校长、北京大学第三医院院长乔杰，北京大学第一医院感染科主任王贵强等接受媒体采访，做出专业解读。跟随北大医学专家的解读，了解病毒、科学防疫，做好自己健康的第一责任人。",
                url:"https://mp.weixin.qq.com/s/62ImP5QEbfY6JFiAV6zxfg"
              },
              {
                title:"除了黄桃还吃啥？特殊群体如何防？北大专家权威解答！",
                date:"如何提高自身对抗病毒的能力,合理补充营养,增强自身免疫力？孕产妇作为特殊群体,要如何做好健康防护,感染新冠后又该如何处理？",
                url:"https://mp.weixin.qq.com/s/Gf_uaW1N9hamxw_6cYdU0g"
              },
              {
                title:"新型冠状病毒感染的肺炎健康教育手册",
                date:"来源：中国健康教育中心",
                url:"http://www.gov.cn/fuwu/2020-02/10/content_5476794.htm"
              },
              {
                title:"新型冠状病毒的几大真相（传播篇）",
                date:"来源： 中国日报网",
                url:"http://www.gov.cn/xinwen/2020-02/07/content_5476018.htm"
              },
              {
                title:"新型冠状病毒的几大真相（防护篇）",
                date:"来源： 中国日报网",
                url:"http://www.gov.cn/xinwen/2020-02/07/content_5476771.htm"
              },
              {
                title:"疫情当前，老人、儿童、孕产妇等特殊群体如何防护？",
                date:"来源： 中国政府网",
                url:"http://www.gov.cn/xinwen/2020-02/06/content_5475295.htm"
              },
            ],
        }
    },
    created(){
        this.getTips()
        this.$vuetify.theme.dark = false
        console.log(this.dialog)
    },
    methods:{
        toQuestionList(){
        this.$router.push({
          path:"/question",
        })
      },

      toSheet(id){
          this.$store.state.id = id
          this.$router.push({
            path:"/sheet",
          })
      },

      getTips(){
        this.$axios.get('/data/tips').then(res => {
          console.log(res);
          if (res.status !== 200) {
            return this.$message.error('获取失败');
          }
          this.tips = res.data.data
        })
      },

      postQuestion(title, content){
        this.dialog = false;
        console.log(title)
        console.log(content)
        var jsondata={title:title, content:content}
        this.$axios.post('http://121.40.243.17:8081/ask',jsondata).then(res=>{
          console.log(res);
        })
        this.title = "";
        this.content = "";
        this.$message({
          type: 'success',
          message: "提交成功！"
        });
      },
      toLocale(link){
        window.location.href = link
      },
    },
}
</script>

<style scoped>
.el-message ,.el-message--success .el-message__content{
  color: white;
}
.el-message .el-icon-success {
  color: white;
}

.el-message__closeBtn.el-icon-close{
  color: white;
}
</style>
