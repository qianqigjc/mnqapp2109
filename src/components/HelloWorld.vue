<template>
  <div>
    <div class="top">
      <div class="span_group">
        <span class="a">肝度:{{ shuxin.gandu }}</span>
        <span class="a">财力:{{ shuxin.caili }}</span>
        <span class="a">操作:{{ shuxin.caozuo }}</span>
        <span class="a">欧气:{{ shuxin.ouqi }}</span>
      </div>
    </div>
    <div class="middle" @click="add">
      <div v-for="(index, i) of thing" :key="i" class="content">
        第{{ thing.length - i }}天:{{ index }}
      </div>
      <h5>{{ msg }}</h5>
    </div>
    <div class="empty"></div>
    <div class="bottom">
      <div>
        <button :class="btnStyle" @click="btnDazao">打造装备</button>
        <button :class="btnStyle_s" @click="btnShenyuan">刷深渊</button>
        <button :class="btnStyle_b" @click="btnBanzhuan">搬砖</button>
      </div>
      <div>
        <button :class="btnStyle_x" @click="btnXiulian">打桩修炼</button>
        <button :class="btnStyle_t" @click="btnTuanben">团本周常</button>
        <button :class="btnStyle_xy" @click="btnXuyuan">许愿</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      btnStyle: "",
      btnStyle_s: "",
      btnStyle_b: "",
      btnStyle_x: "",
      btnStyle_t: "",
      btnStyle_xy: "",
      min: 0,
      max: 7,
      cishu: 0,
      thing: [],
      dazaoliang: 0,
      xiluoke: 0,
      aozima: 0,
      shuxin: {
        gandu: 0,
        caili: 0,
        caozuo: 0,
        ouqi: 0,
      },
      btn: {
        dazao: false,
        shenyuan: false,
        banzhuan: false,
        xiulian: false,
        tuanben: false,
        xuyuan: false,
      },
      text: {
        text1: [
          {
            0: "你主动离职，理由是耽误打dnf，肝度+10,财力-7",
            1: "你在b站观看了西岚视频，受到启发，操作+3",
            2: "你出了张卡片，可惜不值钱,欧气+1",
            3: "你找了份兼职，玩游戏的时间变少了,肝度-9,财力+8",
            4: "你看了xxbb的直播，忍不住氪金的冲动,财力+6",
            5: "你看了xxbb的直播，学习了他的土豪手法,操作-3",
            6: "你加入聊游戏的扯淡群后，水群水的不亦乐乎,操作+1，肝度-1，欧气-1",
            7: "你的女朋友要求你只能周五周六周日晚上八点到九点玩游戏，卑微的你只能答应并偷偷去网吧玩,肝度-6",
            8: "礼包马上要出了，你为了买礼包开始爆肝，肝度+3",
            9: "本来不打算买礼包的你在看完爆料后顿时觉得真香，并开始疯狂爆肝准备多买几套，肝度+6",
            10: "你为了买双至尊成为人上人，没日没夜的爆肝,肝度+9",
          },
          {
            0: "你增幅上头了，胚子全部碎完,顺便杀了你的小浣熊龙神,财力-10",
            1: "你增幅装备,10分钟后无事发生，只是钱变少了,财力-2",
            2: "你出了张传说卡片，而且比较值钱,财力+1,欧气+2",
            3: "你出了张传说卡片，而且非常值钱,财力+3,欧气+4",
            4: "你沉迷于贴吧和colg的打碟，并且踊跃参与其中,操作-2",
            5: "你今天心情不错,欧气+3",
            6: "你帮助了回归玩家，欧气+2",
            7: "你通过对一位大师进行模仿学习，逐渐领悟玄学的力量,欧气+4",
            8: "你对骗子进行举报,欧气+1",
            9: "你上头了，欧气-2",
            10: "你对群友进行欧气的吸收，欧气+1",
          },
          {
            0: "你把睡觉的一部分时间挤出来,肝度+4",
            1: "你觉得你不够肝，决定把时间挤出来,肝度+2",
            2: "你沉迷于游戏,肝度+3",
            3: "你越来越肝，肝度+1",
            4: "你有点困，刚躺下一会儿就睡着了，肝度-1",
            5: "你的肝有点疼,肝度-2",
            6: "你练了个吃手法的职业，操作+2，肝度+1",
            7: "你在网上找攻略看,操作+1",
            8: "策划要削弱你的职业，你开始摆烂，操作-1",
            9: "最近的活动变多了，你沉迷小游戏,肝度+2,操作-1",
            10: "你去医院体检被查出来肝硬化，肝度-10",
          },
          {
            0: "老马看你太非了，决定给你调高了亿点欧气,欧气+20",
            1: "你玩的职业越来越多，逐渐玩明白了这个游戏,操作+5，肝度+5，财力-5",
            2: "你被野团队友给整不会了，操作-1",
            3: "你被野团队友给整麻了，操作-2",
            4: "你被野团队友破防了，操作-3",
            5: "在贴吧的影响下，你逐渐认为秒不了等于打不过，操作-6",
            6: "你买了一些护肝片,财力-4,肝度+6",
            7: "你重复刷图，慢慢形成了肌肉记忆,肝度+2，操作-2",
            8: "你的能力得到全面发展，全属性+1",
            9: "你的能力迅速提升，效率提高，全属性+2",
            10: "你看猴子直播念咒语,你学着念,却遭到反噬,欧气-5",
          },
          {
            0: "老马看你太欧了，决定让你还债,欧气-5",
            1: "你的职业加强了,你决定氪一波,但总感觉落入了老马的圈套,财力+4",
            2: "今天是个爆肝的好日子,肝度+1",
            3: "你感觉今天运气不错,乘这个机会赶紧启动游戏,欧气+1",
            4: "你氪金买了一些金币,财力+1",
            5: "你今天在修炼场专心练习连招,其他的什么也不做,操作+1",
            6: "新活动小游戏奖励不行，玩法也不好玩，你选择放弃,肝度+3,",
            7: "新活动小游戏奖励不行，玩法也不好玩，你选择放弃,肝度+3",
            8: "你去贴吧水贴，然而只有几个人回复",
            9: "更新了减负以后,你没玩的小号也开始去打单人和引导,不打又觉得亏,感觉更肝了",
            10: "老板给涨工资了，你用多出来的钱氪金，财力+7，欧气+5",
          },
          {
            0: "你学着岚岚子去单刷副本，操作手法和技术突飞猛进,操作+8",
            1: "你果然在c站看到了职业平衡之后的打碟盛世",
            2: "你上班摸鱼水群",
            3: "你上班摸鱼水群",
            4: "平平无奇的一天，平平淡淡才是真",
            5: "今天是个爆肝的好日子,肝度+1",
            6: "今天是个爆肝的好日子,肝度+1",
            7: "你买了一些护肝片,财力-4,肝度+6",
            8: "你的能力得到全面发展，全属性+1",
            9: "你的能力得到全面发展，全属性+1",
            10: "你经常看到欺负逆时针在直播时出金牌和出货,总感觉哪里不对劲,欧气-1",
          },
          {
            0: "你经常看姚策划直播，获得策划的庇护，欧气+4",
            1: "你氪金买了一些金币,财力+1",
            2: "你氪金买了一些金币,财力+1",
            3: "最近的活动变多了，你沉迷小游戏,肝度+2,操作-1",
            4: "最近的活动变多了，你沉迷小游戏,肝度+2,操作-1",
            5: "你的肝有点疼,肝度-2",
            6: "你的肝有点疼,肝度-2",
            7: "你感觉今天运气不错,乘这个机会赶紧启动游戏,欧气+1",
            8: "你感觉今天运气不错,乘这个机会赶紧启动游戏,欧气+1",
            9: "你越来越肝，肝度+1",
            10: "你幸运地在拍卖行捡漏成功,财力+3,欧气+2",
          },
          {
            0: "你感觉今天运气不错,乘这个机会赶紧启动游戏,欧气+1",
            1: "今天是个爆肝的好日子,肝度+1",
            2: "今天是个爆肝的好日子,肝度+1",
            3: "你觉得你不够肝，决定把时间挤出来,肝度+2",
            4: "你觉得你不够肝，决定把时间挤出来,肝度+2",
            5: "你氪金买了一些金币,财力+1",
            6: "你氪金买了一些金币,财力+1",
            7: "你氪金买了一些金币,财力+1",
            8: "你今天在修炼场专心练习连招,其他的什么也不做,操作+1",
            9: "礼包马上要出了，你为了买礼包开始爆肝，肝度+3",
            10: "你增幅装备,10分钟后无事发生，只是钱变少了,财力-2",
          },
          {
            0: "礼包马上要出了，你为了买礼包开始爆肝，肝度+3",
            1: "你加了一个聊游戏的扯淡群后，水群水的不亦乐乎,操作+1，肝度-1，欧气-1",
            2: "你在网上找攻略看,操作+1",
            3: "你出了张卡片，可惜不值钱,欧气+1",
            4: "你出了张卡片，可惜不值钱,欧气+1",
            5: "你的能力得到全面发展，全属性+1",
            6: "你的能力得到全面发展，全属性+1",
            7: "组了个1.9名望的斗圣,时装就像五帝王杂交怪,这就是大佬的审美吗",
            8: "今天的队友穿着雪人像素头,感觉憨憨的",
            9: "当论坛讨论爆发还是续航的时候,你摸索出了持续爆发流",
            10: "你的能力迅速提升，效率提高，全属性+2",
          },
          {
            0: "平平无奇的一天，平平淡淡就是真",
            1: "专属单人模式开放后，彩笔们纷纷表示:都秒不掉只能刮痧，根本打不了!",
            2: "韩服更新后出现的bug国服居然都出现了,我们不生产bug,我们只是bug的搬运工",
            3: "你发现群友都是楠酮",
            4: "你上班摸鱼水群",
            5: "国服更新职业平衡，你进入论坛进行一个瓜的吃",
            6: "韩服更新职业平衡，你进入论坛吃瓜",
            7: "你觉得你不够肝，决定把时间挤出来,肝度+2",
            8: "平平无奇的一天，平平淡淡才是真",
            9: "你今天碰到一个复活币战士,幸亏其他队友没怎么用复活币",
            10: "平平无奇的一天，平平淡淡就是真",
          },
          {
            0: "你用计算器得出最优装备，可惜根本没有相应的神话",
            1: "找团的时候你看到有一个团3个巨龙,团名为'高质量巨龙带队3牛,来1.9的C' ",
            2: "找团时你发现1.7的C喊'来1.8的C',1.8的喊'来1.9的C',卷起来了",
            3: "喇叭上时不时有人晒神话，你有点羡慕",
            4: "你看到喇叭带节奏,你屏蔽不了喇叭，只能默默吃瓜",
            5: "你换上命运歧路套装，顿时觉得歧路天下第一",
            6: "新活动点击即送最高天,笑死!还有最高天留不住的勇士?",
            7: "新活动命运抉择送白金自选，我超！策划从南极搬出来了?",
            8: "新上任的韩服策划决定一个月进行一次职业平衡，尹职平他来了!",
            9: "剑魂增强幅度相比其他职业较小,大伙纷纷吐槽:你见过背着五把剑的辅助吗?/少壮不努力,后跳_ _ _",
            10: "你用计算器得出最优装备，可惜根本没有相应的神话",
          },
        ],
        text2: [
          { a: "你购买了直升礼包,100级才是游戏的开始,财力-1，操作-2，肝度+2" },
          { 0: "afw", 1: "aswfg", 2: "weg", 3: "swfg", 4: "yhfg" },
        ],
      },
    };
  },
  methods: {
    add() {
      let numberOne = Math.round(Math.random() * 10);
      let numberTwo = Math.round(Math.random() * 10);
      let obj = this.text.text1[numberOne];
      let arr = this.thing;
      let sx = this.shuxin;
      let t = this.text.text1;
      let btn = this.btn;
      if (arr.length < 2 && this.shuxin.caili > 8) {
        arr.splice(0, 1, this.text.text2[0].a);
        sx.caili -= 1;
        sx.caozuo -= 2;
        sx.gandu += 2;
      }
      if (
        btn.dazao ||
        btn.shenyuan ||
        btn.banzhuan ||
        btn.xiulian ||
        btn.tuanben ||
        btn.xuyuan
      ) {
        if (btn.dazao) {
          let sx = this.shuxin;
          let cl = Math.round(Math.random() * sx.caili);
          let oq = Math.round(Math.random() * sx.ouqi);
          if (sx.caili <= 0) {
            alert("你没有财力进行打造,快去搬砖!");
            return;
          }
          if (cl <= 3) {
            this.thing.unshift("你进行了细节方面的打造,打造+1,财力-1");
            this.dazaoliang += 1;
            sx.caili -= 1;
          } else if (cl > 3 && cl <= 10) {
            if (oq <= 0) {
              this.thing.unshift("你是个大黑蛋子,增幅失败,打造-2,财力-2");
              this.dazaoliang -= 2;
              sx.caili -= 2;
            } else if (oq >= 1 && oq <= 5) {
              this.thing.unshift(
                "你增幅失败后又成功回到原来的增幅等级,感觉无事发生,财力-2"
              );
              sx.caili -= 2;
            } else {
              this.thing.unshift("你增幅成功了,打造+2,财力-2");
              this.dazaoliang += 2;
              sx.caili -= 2;
            }
          } else if (cl > 10 && cl <= 30) {
            if (oq <= 0) {
              this.thing.unshift("你是个大黑蛋子,增幅失败,打造-5,财力-5");
              this.dazaoliang -= 5;
              sx.caili -= 5;
            } else if (oq >= 1 && oq < 16) {
              this.thing.unshift("你增幅失败了,打造-2,财力-5");
              this.dazaoliang -= 2;
              sx.caili -= 5;
            } else if (oq >= 16 && oq <= 30) {
              this.thing.unshift("你增幅成功了!打造+3,财力-5");
              this.dazaoliang += 3;
              sx.caili -= 5;
            } else {
              this.thing.unshift("你增幅取得大成功!打造+5,财力-5");
              this.dazaoliang += 5;
              sx.caili -= 5;
            }
          } else if (cl > 30 && cl <= 100) {
            if (oq <= 0) {
              this.thing.unshift("你是个大黑蛋子,增幅失败,打造-8,财力-10");
              this.dazaoliang -= 8;
              sx.caili -= 10;
            } else if (oq >= 1 && oq < 20) {
              this.thing.unshift("你增幅失败了,打造-5,财力-10");
              this.dazaoliang -= 5;
              sx.caili -= 10;
            } else if (oq >= 20 && oq < 40) {
              this.thing.unshift("你增幅失败了,打造-2,财力-7");
              this.dazaoliang -= 2;
              sx.caili -= 7;
            } else if (oq >= 40 && oq <= 60) {
              this.thing.unshift("你增幅成功了!打造+5,财力-8");
              this.dazaoliang += 5;
              sx.caili -= 8;
            } else {
              this.thing.unshift("你增幅取得大成功!打造+10,财力-10");
              this.dazaoliang += 10;
              sx.caili -= 10;
            }
          } else if (cl > 100) {
            if (oq <= 0) {
              this.thing.unshift("你是个大黑蛋子,增幅失败,打造-10,财力-20");
              this.dazaoliang -= 10;
              sx.caili -= 20;
            } else if (oq >= 1 && oq < 20) {
              this.thing.unshift("你增幅失败了,打造-6,财力-20");
              this.dazaoliang -= 6;
              sx.caili -= 20;
            } else if (oq >= 20 && oq < 40) {
              this.thing.unshift("你增幅失败了,打造-3,财力-15");
              this.dazaoliang -= 3;
              sx.caili -= 15;
            } else if (oq >= 40 && oq < 60) {
              this.thing.unshift(
                "你增幅失败了,你花了额外的金币回到原来的打造,财力-25"
              );
              sx.caili -= 25;
            } else if (oq >= 60 && oq < 100) {
              this.thing.unshift(
                "你增幅成功了!胚子成了几个红11和一个红12!打造+15,财力-20"
              );
              this.dazaoliang += 15;
              sx.caili -= 20;
            } else if (oq >= 100 && oq <= 160) {
              this.thing.unshift(
                "你增幅成功了!胚子成了几个红12和一个红13!打造+25,财力-20"
              );
              this.dazaoliang += 25;
              sx.caili -= 20;
            } else {
              this.thing.unshift(
                "你连碎几个垫子后丢红12的胚子,居然成了几件红13!打造+35,财力-20"
              );
              this.dazaoliang += 35;
              sx.caili -= 20;
            }
          }
        }
        if (btn.shenyuan) {
          let sx = this.shuxin;
          let ba = Math.floor(sx.gandu / 2);
          let oq = sx.ouqi / 20;
          if (sx.gandu <= 0) {
            alert("你太累了不能刷深渊,快去养养肝吧");
            return;
          }
          if (ba > 20) {
            ba = 20;
          }
          let ss = ba * oq * 0.2;
          let ge = Math.round(Math.random() * ss);
          let shenhualv = 1000 / ba / oq;
          let shenhua = Math.round(Math.random() * shenhualv);
          if (shenhua == 0 || shenhua == shenhualv) {
            this.thing.unshift(
              "深渊柱子缓缓从金色变为彩色,是斯宾塞!出神话啦!你根据神话进行装备搭配,战斗力惊人地上涨了50点,肝度+5"
            );
            this.dazaoliang += 50;
            sx.gandu += 5;
            return;
          }
          if (ge <= 0) {
            this.thing.unshift(
              "你光头了,据说欧气为20的时候,每把出货率为10%,快去提升欧气吧,肝度-2"
            );
            sx.gandu -= 2;
          } else {
            let er = Math.round(Math.random() * oq);
            if (er == 0) {
              er = oq;
            } else if (er == 1) {
              er = 1.5;
            }
            let wuqi = Math.floor(ge / er / 2);
            let tishen = ge - wuqi;
            sx.gandu -= 2;
            this.dazaoliang += tishen;
            this.thing.unshift(
              `你刷了${ba}把深渊,出了${ge}个史诗,其中重复的和其他职业的武器有${wuqi}个,打造提升${tishen}点,肝度-2`
            );
          }
        }
        if (btn.banzhuan) {
          if (this.shuxin.gandu <= 0) {
            alert("你太累了搬不了砖!快去养养肝吧");
            return;
          }
          this.thing.unshift("你努力搬了一天的砖,财力+2,肝度-1");
          this.shuxin.caili += 2;
          this.shuxin.gandu -= 1;
        }
        if (btn.xiulian) {
          if (this.shuxin.gandu <= 0) {
            alert("快去睡觉吧别玩了!");
            return;
          }
          let shanghai = this.dazaoliang / 10;
          this.thing.unshift(
            `你在修炼场练习连招,顺便测了下伤害,20秒${shanghai}亿,操作+1,肝度-1`
          );
          this.shuxin.caozuo += 1;
          this.shuxin.gandu -= 1;
        }
        if (btn.tuanben) {
          let n = Math.round(Math.random() * 2);
          let l = Math.round(Math.random() * 2);
          let c = this.shuxin.caozuo / 20 + this.dazaoliang / 20;
          let cz = Math.round(Math.random() * c);
          let jinpai = 20 / (this.shuxin.ouqi / 40);
          let jinplv = Math.round(Math.random() * jinpai);
          if (this.shuxin.gandu < 10) {
            alert("别肝了!团是永远也打不完的(肝度低于10无法打团)");
            return;
          }
          if (this.xiluoke >= 182) {
            this.thing.unshift(
              "你消耗了182个花瓣开罐子,并进行装备升级,打造+30,花瓣-182"
            );
            this.xiluoke -= 182;
            this.dazaoliang += 30;
            return;
          } else if (this.aozima >= 242) {
            this.thing.unshift(
              "你消耗了242个奥兹玛材料开罐子,并进行装备升级,打造+40,材料-242"
            );
            this.aozima -= 242;
            this.dazaoliang += 40;
            return;
          }
          if (this.dazaoliang <= 0) {
            alert("你什么装备也没有,打不了团");
            return;
          } else if (this.dazaoliang > 0 && this.dazaoliang < 20) {
            this.thing.unshift("希洛克团本,你申请了很多个团,但根本没有人放你");
          } else if (this.dazaoliang >= 20 && this.dazaoliang < 30) {
            if (n == 1) {
              this.thing.unshift(
                "希洛克团本,你进了只有几个人的团,还差一个人开团的时候,你被强制退出攻坚队"
              );
            } else {
              if (cz == 0 || cz == c) {
                this.thing.unshift(
                  "希洛克团本,可能因为你或你的队友的伤害太低或操作太菜,你炸团了"
                );
              } else {
                if (jinplv == 0 || jinplv == jinpai) {
                  this.thing.unshift(
                    "希洛克团本攻坚成功,你翻到了金牌!希洛克装备+1,升级后打造+30"
                  );
                  this.dazaoliang += 30;
                } else {
                  this.thing.unshift("希洛克团本攻坚成功,获得18个花瓣");
                  this.xiluoke += 18;
                }
              }
            }
          } else if (this.dazaoliang >= 30 && this.dazaoliang < 50) {
            if (n == 0 && this.shuxin.ouqi < 40) {
              this.thing.unshift(
                "希洛克团本,你进了只有几个人的团,还差一个人开团的时候,你被强制退出攻坚队"
              );
            } else {
              if (cz == 0) {
                this.thing.unshift(
                  "希洛克团本,可能因为你或你的队友的伤害太低或操作太菜,你炸团了"
                );
              } else {
                if (jinplv == 0 || jinplv == jinpai) {
                  this.thing.unshift(
                    "希洛克团本攻坚成功,你翻到了金牌!希洛克装备+1,升级后打造+30"
                  );
                  this.dazaoliang += 30;
                } else {
                  this.thing.unshift("希洛克团本攻坚成功,获得18个花瓣");
                  this.xiluoke += 18;
                }
              }
            }
          } else if (this.dazaoliang >= 50 && this.dazaoliang < 80) {
            if (n == 1) {
              if (jinplv == 0 || jinplv == jinpai) {
                this.thing.unshift(
                  "希洛克团本攻坚成功,你翻到了金牌!希洛克装备+1,升级后打造+30"
                );
                this.dazaoliang += 30;
              } else {
                this.thing.unshift("希洛克团本攻坚成功,获得18个花瓣");
                this.xiluoke += 18;
              }
            } else {
              if (l == 1) {
                this.thing.unshift(
                  "奥兹玛团本1牛,你进了只有几个人的团,还差一个人开团的时候,你被强制退出攻坚队"
                );
              } else {
                if (cz == 0 || cz == c || cz == 1 || cz == c - 1) {
                  this.thing.unshift(
                    "奥兹玛团本1牛,可能因为你或你的队友的伤害太低或操作太菜,你炸团了"
                  );
                } else {
                  if (jinplv == 0 || jinplv == jinpai) {
                    this.thing.unshift(
                      "奥兹玛团本1牛攻坚成功,你翻到了金牌!奥兹玛装备+1,升级后打造+40"
                    );
                    this.dazaoliang += 40;
                  } else {
                    this.thing.unshift("奥兹玛团本1牛攻坚成功,获得26个材料");
                    this.aozima += 26;
                  }
                }
              }
            }
          } else if (this.dazaoliang >= 80 && this.dazaoliang < 120) {
            if (n == 1) {
              if (jinplv == 0 || jinplv == jinpai) {
                this.thing.unshift(
                  "奥兹玛团本1牛攻坚成功,你翻到了金牌!奥兹玛装备+1,升级后打造+40"
                );
                this.dazaoliang += 40;
              } else {
                this.thing.unshift("奥兹玛团本1牛攻坚成功,获得26个材料");
                this.aozima += 26;
              }
            } else {
              if (l == 1) {
                this.thing.unshift(
                  "奥兹玛团本2牛,你进了只有几个人的团,还差一个人开团的时候,你被强制退出攻坚队"
                );
              } else {
                if (cz == 0 || cz == c || cz == 1 || cz == c - 1) {
                  this.thing.unshift(
                    "奥兹玛团本2牛,可能因为你或你的队友的伤害太低或操作太菜,你炸团了"
                  );
                } else {
                  if (jinplv == 0 || jinplv == jinpai) {
                    this.thing.unshift(
                      "奥兹玛团本2牛攻坚成功,你翻到了金牌!奥兹玛装备+1,升级后打造+40"
                    );
                    this.dazaoliang += 40;
                  } else {
                    this.thing.unshift("奥兹玛团本2牛攻坚成功,获得36个材料");
                    this.aozima += 36;
                  }
                }
              }
            }
          } else if (this.dazaoliang >= 120) {
            if (n == 1) {
              if (jinplv == 0 || jinplv == jinpai) {
                this.thing.unshift(
                  "奥兹玛团本2牛攻坚成功,你翻到了金牌!奥兹玛装备+1,升级后打造+40"
                );
                this.dazaoliang += 40;
              } else {
                this.thing.unshift("奥兹玛团本2牛攻坚成功,获得36个材料");
                this.aozima += 36;
              }
            } else {
              if (cz == 0 || cz == c || cz == 1 || cz == c - 1) {
                this.thing.unshift(
                  "奥兹玛团本3牛,可能因为你或你的队友的伤害太低或操作太菜,你炸团了"
                );
              } else {
                if (jinplv == 0 || jinplv == jinpai) {
                  this.thing.unshift(
                    "奥兹玛团本3牛攻坚成功,你翻到了金牌!奥兹玛装备+1,升级后打造+40"
                  );
                  this.dazaoliang += 40;
                } else {
                  this.thing.unshift("奥兹玛团本3牛攻坚成功,获得50个材料");
                  this.aozima += 50;
                }
              }
            }
          }
          let min = this.thing.length - (this.thing.length % 7);
          let max = min + 7;
          if (min == 0) {
            this.thing.shift();
            alert("你还是萌新,团本还是以后再说吧");
            return;
          } else if (min != this.min) {
            this.min = min;
            this.max = max;
            this.cishu = 1;
          } else if (min == this.min && this.cishu == 1) {
            this.cishu = 2;
          } else {
            this.thing.shift();
            alert("一周只能打两次团,下周再来吧");
            return;
          }
        }
        if (btn.xuyuan) {
          if (this.shuxin.caili <= 0) {
            alert("没有钱就无法获得欧气,老马并不喜欢穷比");
            return;
          }
          let num = Math.round(Math.random() * 2);
          if (num == 0) {
            this.thing.unshift(
              "你的钱白花了,欧气并没有上涨,可能是给少了的缘故,财力-1"
            );
            this.shuxin.caili -= 1;
          } else if (num == 1) {
            this.thing.unshift(
              "你的钱诚打动了老马,并给你调高了概率,欧气+1,财力-1"
            );
            this.shuxin.ouqi += 1;
            this.shuxin.caili -= 1;
          } else {
            this.thing.unshift(
              "你的钱诚打动了老马,给你大幅调高了概率,欧气+2,财力-1"
            );
            this.shuxin.ouqi += 2;
            this.shuxin.caili -= 1;
          }
        }
        return;
      }
      arr.unshift(obj[numberTwo]);
      if (arr[0] == t[0][0]) {
        sx.gandu += 10;
        sx.caili -= 7;
      } else if (arr[0] == t[0][1]) {
        sx.caozuo += 3;
      } else if (arr[0] == t[0][2]) {
        sx.ouqi += 1;
      } else if (arr[0] == t[0][3]) {
        sx.gandu -= 9;
        sx.caili += 8;
      } else if (arr[0] == t[0][4]) {
        sx.caili += 6;
      } else if (arr[0] == t[0][5]) {
        sx.caozuo -= 3;
      } else if (arr[0] == t[0][6]) {
        sx.caozuo += 1;
        sx.gandu -= 1;
        sx.ouqi -= 1;
      } else if (arr[0] == t[0][7]) {
        sx.gandu -= 6;
      } else if (arr[0] == t[0][8]) {
        sx.gandu += 3;
      } else if (arr[0] == t[0][9]) {
        sx.gandu += 6;
      } else if (arr[0] == t[0][10]) {
        sx.gandu += 9;
      } else if (arr[0] == t[1][0]) {
        sx.caili -= 10;
      } else if (arr[0] == t[1][1]) {
        sx.caili -= 2;
      } else if (arr[0] == t[1][2]) {
        sx.caili += 1;
        sx.ouqi += 2;
      } else if (arr[0] == t[1][3]) {
        sx.caili += 1;
        sx.ouqi += 2;
      } else if (arr[0] == t[1][4]) {
        sx.caozuo -= 2;
      } else if (arr[0] == t[1][5]) {
        sx.ouqi += 3;
      } else if (arr[0] == t[1][6]) {
        sx.ouqi += 2;
      } else if (arr[0] == t[1][7]) {
        sx.ouqi += 4;
      } else if (arr[0] == t[1][8]) {
        sx.ouqi += 1;
      } else if (arr[0] == t[1][9]) {
        sx.ouqi -= 2;
      } else if (arr[0] == t[1][10]) {
        sx.ouqi += 1;
      } else if (arr[0] == t[2][0]) {
        sx.gandu += 4;
      } else if (arr[0] == t[2][1]) {
        sx.gandu += 2;
      } else if (arr[0] == t[2][2]) {
        sx.gandu += 3;
      } else if (arr[0] == t[2][3]) {
        sx.gandu += 1;
      } else if (arr[0] == t[2][4]) {
        sx.gandu -= 1;
      } else if (arr[0] == t[2][5]) {
        sx.gandu -= 2;
      } else if (arr[0] == t[2][6]) {
        sx.caozuo += 2;
        sx.gandu += 1;
      } else if (arr[0] == t[2][7]) {
        sx.caozuo += 1;
      } else if (arr[0] == t[2][8]) {
        sx.caozuo -= 1;
      } else if (arr[0] == t[2][9]) {
        sx.gandu += 2;
        sx.caozuo -= 1;
      } else if (arr[0] == t[2][10]) {
        sx.gandu -= 10;
      } else if (arr[0] == t[3][0]) {
        sx.ouqi += 20;
      } else if (arr[0] == t[3][1]) {
        sx.caozuo += 5;
        sx.gandu += 5;
        sx.caili -= 5;
      } else if (arr[0] == t[3][2]) {
        sx.caozuo -= 1;
      } else if (arr[0] == t[3][3]) {
        sx.caozuo -= 2;
      } else if (arr[0] == t[3][4]) {
        sx.caozuo -= 3;
      } else if (arr[0] == t[3][5]) {
        sx.caozuo -= 6;
      } else if (arr[0] == t[3][6]) {
        sx.caili -= 4;
        sx.gandu += 6;
      } else if (arr[0] == t[3][7]) {
        sx.gandu += 2;
        sx.caozuo -= 2;
      } else if (arr[0] == t[3][8]) {
        sx.gandu += 1;
        sx.caozuo += 1;
        sx.caili += 1;
        sx.ouqi += 1;
      } else if (arr[0] == t[3][9]) {
        sx.gandu += 2;
        sx.caozuo += 2;
        sx.caili += 2;
        sx.ouqi += 2;
      } else if (arr[0] == t[3][10]) {
        sx.ouqi -= 5;
      } else if (arr[0] == t[4][0]) {
        sx.ouqi -= 5;
      } else if (arr[0] == t[4][1]) {
        sx.caili += 4;
      } else if (arr[0] == t[4][2]) {
        sx.gandu += 1;
      } else if (arr[0] == t[4][3]) {
        sx.ouqi += 1;
      } else if (arr[0] == t[4][4]) {
        sx.caili += 1;
      } else if (arr[0] == t[4][5]) {
        sx.caozuo += 1;
      } else if (arr[0] == t[4][6]) {
        sx.gandu += 3;
      } else if (arr[0] == t[4][10]) {
        sx.caili += 7;
        sx.ouqi += 5;
      } else if (arr[0] == t[5][0]) {
        sx.caozuo += 8;
      } else if (arr[0] == t[5][10]) {
        sx.ouqi -= 1;
      } else if (arr[0] == t[6][0]) {
        sx.ouqi += 4;
      } else if (arr[0] == t[6][10]) {
        sx.caili += 3;
        sx.ouqi += 2;
      }
    },
    btnDazao() {
      let sx = this.shuxin;
      if (sx.caili <= 0) {
        this.btnStyle = "";
        this.btn.dazao = false;
        alert("你没有财力进行打造,快去搬砖!");
        return;
      }
      if (this.btnStyle) {
        this.btnStyle = "";
        this.btn.dazao = false;
      } else {
        this.btnStyle = "btn_click";
        this.btn.dazao = true;
      }
    },
    btnShenyuan() {
      let sx = this.shuxin;
      if (sx.gandu <= 0) {
        this.btnStyle_s = "";
        this.btn.shenyuan = false;
        alert("你太累了不能刷深渊,快去养养肝吧");
        return;
      }
      if (this.btnStyle_s) {
        this.btnStyle_s = "";
        this.btn.shenyuan = false;
      } else {
        this.btnStyle_s = "btn_click";
        this.btn.shenyuan = true;
      }
    },
    btnBanzhuan() {
      let sx = this.shuxin;
      if (sx.gandu <= 0) {
        this.btnStyle_b = "";
        this.btn.banzhuan = false;
        alert("你太累了搬不了砖!快去养养肝吧");
        return;
      }
      if (this.btnStyle_b) {
        this.btnStyle_b = "";
        this.btn.banzhuan = false;
      } else {
        this.btnStyle_b = "btn_click";
        this.btn.banzhuan = true;
      }
    },
    btnXiulian() {
      let sx = this.shuxin;
      if (sx.gandu <= 0) {
        this.btnStyle_x = "";
        this.btn.xiulian = false;
        alert("快去睡觉吧别玩了!");
        return;
      }
      if (this.btnStyle_x) {
        this.btnStyle_x = "";
        this.btn.xiulian = false;
      } else {
        this.btnStyle_x = "btn_click";
        this.btn.xiulian = true;
      }
    },
    btnTuanben() {
      let sx = this.shuxin;
      if (sx.gandu < 10) {
        this.btnStyle_t = "";
        this.btn.tuanben = false;
        alert("别肝了!团是永远也打不完的(肝度低于10无法打团)");
        return;
      }
      if (this.btnStyle_t) {
        this.btnStyle_t = "";
        this.btn.tuanben = false;
      } else {
        this.btnStyle_t = "btn_click";
        this.btn.tuanben = true;
      }
    },
    btnXuyuan() {
      let sx = this.shuxin;
      if (sx.caili <= 0) {
        this.btnStyle_xy = "";
        this.btn.xuyuan = false;
        alert("没有钱就无法获得欧气,老马并不喜欢穷比");
        return;
      }
      if (this.btnStyle_xy) {
        this.btnStyle_xy = "";
        this.btn.xuyuan = false;
      } else {
        this.btnStyle_xy = "btn_click";
        this.btn.xuyuan = true;
      }
    },
  },
  created() {
    this.shuxin.gandu = Math.round(Math.random() * 10);
    this.shuxin.caili = Math.round(Math.random() * 10);
    this.shuxin.caozuo = Math.round(Math.random() * 10);
    this.shuxin.ouqi = Math.round(Math.random() * 10);
  },
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.top {
  width: 100%;
  height: 50px;
  background-color: #eef;
  position: fixed;
  top: 0;
  margin-left: -8px;
}
.bottom {
  width: 100%;
  height: 80px;
  background-color: #eef;
  position: fixed;
  bottom: 0;
  margin-left: -8px;
  background-color: #eef;
}
.empty {
  width: 355px;
  height: 80px;
  background-color: #fff;
}
.middle {
  width: 355px;
  height: auto;
  border: 2px solid #666;
  border-radius: 10px;
  margin: 0 auto;
}
.content {
  width: 100%;
  padding: 10px 0;
  border-bottom: 1px solid #aaa;
}
.span_group {
  margin-top: 18px;
}
.a {
  border: 2px solid #daa;
  border-radius: 6px;
  margin: 8px;
  padding: 5px;
}
button {
  width: 90px;
  padding: 5px;
  margin: 5px;
}
.btn_click {
  background-color: #eee;
  color: rgb(33, 83, 141);
  border: 2px solid #daa;
  border-radius: 3px;
}
</style>
