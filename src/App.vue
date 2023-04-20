<template>
  <div class="app">
    <div class="sumup">
      <div class="wrap">
        <div class="costPerformance">
          <div class="title">工作性价比指数</div>
          <div class="num">
            {{ costPerformance }}
          </div>
          <div class="desc">
            <div class="dot"></div>
            {{ costPerformanceDesc }}
          </div>
        </div>
      </div>
      <div class="wrap">
        <div class="softCompositeIndex">
          <div class="title">软性综合指数</div>
          <div class="num">
            {{ softCompositeIndex }}
          </div>
          <div class="desc">
            <div class="dot"></div>
            {{ softCompositeIndexDesc }}
          </div>
        </div>
      </div>
    </div>
    <div class="hardcoreTitle">硬性综合</div>
    <div class="wrap">
      <div class="hardcore">
        <div class="item">
          <div class="title">到手月薪/元</div>
          <div class="inputNum">
            <input type="number" v-model="salary" />
          </div>
          <div class="desc">
            {{ salaryDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">事实工时/h/月</div>
          <div class="inputNum">
            <input type="number" v-model="workTime" />
          </div>
          <div class="desc">
            {{ workTimeDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">通勤成本/h/天</div>
          <div class="inputNum">
            <input type="number" v-model="transport" />
          </div>
          <div class="desc">
            {{ transportDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">工龄/年</div>
          <div class="inputNum">
            <input type="number" v-model="workAge" />
          </div>
          <div class="desc">
            {{ workAgeDesc }}
          </div>
        </div>
      </div>
    </div>
    <div class="softcoreTitle">软性综合</div>
    <div class="wrap">
      <div class="hardcore">
        <div class="item">
          <div class="title">工作城市</div>
          <div class="inputNum">
            <el-slider v-model="city" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ cityDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">生活环境</div>
          <div class="inputNum">
            <el-slider
              v-model="environment"
              size="small"
              :max="10"
              show-stops
            />
          </div>
          <div class="desc">
            {{ environmentDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">工作压力</div>
          <div class="inputNum">
            <el-slider v-model="pressure" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ pressureDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">学历条件</div>
          <div class="inputNum">
            <el-slider v-model="education" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ educationDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">同事氛围</div>
          <div class="inputNum">
            <el-slider v-model="colleague" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ colleagueDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">通勤氛围</div>
          <div class="inputNum">
            <el-slider v-model="commute" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ commuteDesc }}
          </div>
        </div>
        <div class="item">
          <div class="title">早八</div>
          <div class="inputNum">
            <el-slider v-model="morning" size="small" :max="10" show-stops />
          </div>
          <div class="desc">
            {{ morningDesc }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      salary: 3350,
      workTime: 160,
      transport: 0,
      workAge: 0,
      city: 5,
      environment: 5,
      pressure: 5,
      education: 5,
      colleague: 5,
      commute: 5,
      morning: 5,
    };
  },
  computed: {
    costPerformanceDesc() {
      if (this.costPerformance <= 0.9) {
        return "惨绝人寰";
      } else if (this.costPerformance <= 1.1) {
        return "惨";
      } else if (this.costPerformance <= 1.3) {
        return "一般般惨";
      } else if (this.costPerformance <= 1.5) {
        return "普通社畜";
      } else if (this.costPerformance <= 1.7) {
        return "有点爽";
      }
      return "哪个部门，介绍一下？";
    },
    softCompositeIndexDesc() {
      if (this.softCompositeIndex <= 0.8) {
        return "有点糟糕";
      } else if (this.softCompositeIndex <= 1) {
        return "这份工作会为你带来负面情绪";
      } else if (this.softCompositeIndex <= 1.2) {
        return "一般般，也许你会想要更好的";
      } else if (this.softCompositeIndex <= 1.4) {
        return "挺不错的，不是吗";
      } else if (this.softCompositeIndex <= 1.6) {
        return "十分让人羡慕";
      }
      return "太厉害啦，你为自己争取到了几乎顶尖的工作环境！";
    },
    costPerformance() {
      return (
        ((this.environmentCoefficient *
          this.colleagueCoefficient ** 0.5 *
          this.morningCoefficient ** 0.5) /
          this.cityCoefficient ** 2 /
          this.pressureCoefficient /
          this.educationCoefficient /
          this.workAgeCoefficient) *
        (this.salary /
          (this.workTime +
            (this.transport * this.workTime) / 8 / this.commuteCoefficient) /
          23.05) **
          0.5 *
        (160 / this.workTime) ** 2
      ).toFixed(2);
    },
    softCompositeIndex() {
      return (
        ((this.cityCoefficient * this.environmentCoefficient) /
          this.pressureCoefficient ** 2) *
        this.educationCoefficient *
        this.colleagueCoefficient *
        this.commuteCoefficient ** 0.5 *
        this.morningCoefficient ** 0.7
      ).toFixed(2);
    },
    salaryDesc() {
      if (this.salary <= 3760.25) {
        return "给国家拖后腿了实在抱歉";
      } else if (this.salary <= 10 * 3760.25) {
        return "至少比基准值高，不是吗";
      }
      return "已经是人上人了";
    },
    salaryCoefficient() {
      return (this.salary / 3760.25) ** 0.5;
    },
    workTimeDesc() {
      if (this.workTime <= 160) {
        return "厉害了，每天工作不超过8小时";
      } else if (this.workTime <= 168) {
        return "平均每月加班一天，还可以";
      } else if (this.workTime <= 176) {
        return "要么大小周，要么隔天就加一会儿班";
      } else if (this.workTime <= 200) {
        return "天天加班？有点猛啊兄弟";
      }
      return "哪个部门排个雷？这是违反劳动法的程度";
    },
    workTimeCoefficient() {
      return (this.workTime / 160) ** 2;
    },
    transportDesc() {
      if (this.transport <= 0.6) {
        return "很舒服，我猜你的通勤方式是走路或骑车";
      } else if (this.transport <= 1) {
        return "还好，也许是平均水平";
      } else if (this.transport <= 2) {
        return "可能会有一点累";
      } else if (this.transport <= 4) {
        return "跨省通勤？";
      }
      return "你的生活不会除了工作和睡觉就是通勤吧？";
    },
    workAgeDesc() {
      if (this.workAge <= 1) {
        return "同学你好";
      } else if (this.workAge <= 3) {
        return "时间过得真快不是吗";
      }
      return "hey老兵";
    },
    workAgeCoefficient() {
      return Math.log10(9 + this.workAge);
    },
    cityDesc() {
      if (this.city <= 5) {
        return "也许你在小镇上班";
      } else if (this.city <= 7) {
        return "二三线城市的样子";
      } else if (this.city <= 9) {
        return "一线城市也许会是9左右";
      }
      return "超一线城市，非常繁荣的地方，这里的房价很高";
    },
    cityCoefficient() {
      return Math.log10(5 + this.city);
    },
    environmentDesc() {
      if (this.environment <= 3) {
        return "艰苦卓绝";
      } else if (this.environment <= 5) {
        return "鸟不拉屎";
      } else if (this.environment <= 7) {
        return "外卖还是有的，就是要花点配送费";
      } else if (this.environment <= 9) {
        return "只是想逛街的话，不需要跑太远";
      }
      return "楼下就是CBD";
    },
    environmentCoefficient() {
      return this.environment / 20 + 0.75;
    },
    pressureDesc() {
      if (this.pressure <= 3) {
        return "轻松，闲适，压力与你无关";
      } else if (this.pressure <= 5) {
        return "按部就班的工作";
      } else if (this.pressure <= 7) {
        return "有一些压力，富有节奏感的工作";
      } else if (this.pressure <= 9) {
        return "有不小的压力，偶尔会失眠";
      }
      return "在靠近一点点就会爆炸！";
    },
    pressureCoefficient() {
      return this.pressure / 20 + 0.75;
    },
    educationDesc() {
      if (this.education <= 3) {
        return "本科及以下";
      } else if (this.education <= 4) {
        return "一本";
      } else if (this.education <= 6) {
        return "211，或者Top200";
      } else if (this.education <= 7) {
        return "985、Top100或者普通硕士";
      } else if (this.education <= 9) {
        return "985、Top100以及硕士";
      }
      return "博士";
    },
    educationCoefficient() {
      return this.education / 20 + 0.75;
    },
    colleagueDesc() {
      if (this.colleague <= 3) {
        return "恕我直言，同事都是傻逼";
      } else if (this.colleague <= 5) {
        return "虽然有一些笨蛋，但还行";
      } else if (this.colleague <= 7) {
        return "大家都很普通，或是好坏参半";
      } else if (this.colleague <= 9) {
        return "同事们都很优秀，性格很好";
      }
      return "大家都是人才，说话也好听，待在公司不想回家";
    },
    colleagueCoefficient() {
      return this.colleague / 20 + 0.75;
    },
    commuteDesc() {
      if (this.commute <= 3) {
        return "堵车非常严重，要怒路了！";
      } else if (this.commute <= 5) {
        return "堵车，或者公共交通没座位，总之有点难受";
      } else if (this.commute <= 7) {
        return "一般，普通的早高峰";
      } else if (this.commute <= 9) {
        return "同事都羡慕我能够轻松通勤";
      }
      return "住公司是吧？";
    },
    commuteCoefficient() {
      return this.commute / 20 + 0.75;
    },
    morningDesc() {
      if (this.morning <= 3) {
        return "什么工种啊六七点上班";
      } else if (this.morning <= 5) {
        return "八点打卡才叫早八";
      } else if (this.morning <= 7) {
        return "九点打卡";
      } else if (this.morning <= 9) {
        return "十点打卡";
      }
      return "笑死，根本不需要打卡";
    },
    morningCoefficient() {
      return this.morning / 20 + 0.75;
    },
  },
};
</script>

<style lang="scss">
$theme: #6963eb;
.app {
  padding: 20px;
}
.sumup {
  display: flex;
  position: sticky;
  top: 0;
  background-color: white;
  z-index: 10;
}
.wrap {
  width: 100%;
  box-shadow: 3px 6px 20px rgba($color: #000000, $alpha: 0.2);
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 20px;
  &:first-of-type {
    margin-right: 20px;
  }
  .costPerformance {
    .title {
      font-size: 12px;
      color: darkgray;
      margin-bottom: 5px;
    }
    .num {
      color: $theme;
      font-size: 48px;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
        Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
        sans-serif;
    }
    .desc {
      padding-top: 10px;
      font-size: 14px;
      display: flex;
      .dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background-color: $theme;
        margin-right: 6px;
        flex-shrink: 0;
        margin-top: 5px;
      }
    }
  }
  .softCompositeIndex {
    @extend .costPerformance;
  }
  .hardcore {
    .item {
      margin-bottom: 20px;
      .title {
        font-size: 13px;
        margin-bottom: 10px;
      }
      .desc {
        margin-top: 6px;
        font-size: 12px;
        color: gray;
        text-align: right;
      }
    }
  }
}
.hardcoreTitle,
.softcoreTitle {
  margin-bottom: 15px;
  color: gray;
  font-size: 13px;
}

input[type="number"] {
  border: 2px solid #6963eb;
  width: 100%;
  height: 40px;
  border-radius: 10px;
  padding-left: 10px;
}
.el-slider__bar {
  background-color: $theme !important;
}
.el-slider__button {
  border: 2px solid $theme !important;
}
</style>
