<template>
  <div class="content">
    <div class="wrapper">
      <div>{{ displayNum }}</div>
      <ul class="row_ul">
        <li v-for="(row, rowIndex) in rows" :key="rowIndex">
          <ul class="item_ul">
            <li
              v-for="(item, itemIndex) in row"
              :key="itemIndex"
              :class="{ active: item === activeRow }"
              @click="onClick(item)"
            >
              {{ item }}
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      prevNum: "0",
      nextNum: null,
      operator: '',
      activeRow: "",
      // 使用Object.freeze，vue不会对items里的object做getter、setter绑定，可优化速度，提升性能
      rows: Object.freeze({
        row1: ["AC", "DEL", "%", "/"],
        row2: [7, 8, 9, "*"],
        row3: [4, 5, 6, "-"],
        row4: [1, 2, 3, "+"],
        row5: [0, ".", "="],
      }),
    };
  },
  computed: {
    displayNum() {
      const { prevNum, nextNum } = this;
      return nextNum ? nextNum : prevNum;
    }
  },
  methods: {
    onClick(content) {
      this.activeRow = content;
      if (typeof content === "number") {
        if (this.operator === '=') {
          this.prevNum = content + '';
          this.operator = '';
          return;
        }
        if (['+', '-', '*', '/', '%'].includes(this.operator)) {
          this.nextNum += content;
          // 去掉首位数字为0
          this.nextNum = parseFloat(this.nextNum) + "";
        } else {
          this.prevNum += content;
          // 去掉首位数字为0
          this.prevNum = parseFloat(this.prevNum) + "";
        }
      } else {
        if (['+', '-', '*', '/', '%'].includes(content)) {
          if (this.nextNum) {
            this.calc(content);
          } else {
            this.operator = content;
          }
        } else {
          // this.operator = '';
          switch (content) {
            case "AC":
              this.empty();
              break;
            case "DEL":
              this.delete();
              break;
            case '=':
              this.calc(content);
              break;
            default:
              break;
          }
        }
      }
    },
    // 清空
    empty() {
      this.prevNum = '0';
      this.nextNum = null;
    },
    // 删除
    delete() {
      if (this.nextNum) {
        this.nextNum = this.nextNum.slice(0, this.nextNum.length - 1);
        this.nextNum = !this.nextNum ? '0' : this.nextNum;
      } else {
        this.prevNum = this.prevNum.slice(0, this.prevNum.length - 1);
        this.prevNum = !this.prevNum ? '0' : this.prevNum;
      }
    },
    calc(content) {
      switch (this.operator) {
        case '+':
          this.prevNum = parseFloat(this.prevNum) + parseFloat(this.nextNum);
          break;
        case '-':
          this.prevNum = parseFloat(this.prevNum) - parseFloat(this.nextNum);
          break;
        case '*':
          this.prevNum = parseFloat(this.prevNum) * parseFloat(this.nextNum);
          break;
        case '/':
          this.prevNum = parseFloat(this.prevNum) / parseFloat(this.nextNum);
          break;
        case '%':
          this.prevNum = parseFloat(this.prevNum) % parseFloat(this.nextNum);
          break;
      }
      this.prevNum += '';
      this.nextNum = null;
      this.operator = content;
    },
  },
};
</script>

<style scoped lang="less">
.content {
  height: 100%;
  position: relative;
  font-size: 0.52rem;
  .wrapper {
    padding: 0 0.3rem;
    width: 100%;
    position: absolute;
    bottom: 0;
    left: 0;
    > div {
      text-align: right;
      padding: 0.5rem 0.3rem;
      font-size: 0.8rem;
    }
  }
  .row_ul {
    > li {
      &:last-of-type li:first-of-type {
        width: 3.3rem;
        border-radius: 50rem;
      }
      &:first-of-type {
        li {
          background: grey;
          border: 1px solid grey !important;
        }
      }
      .item_ul {
        display: flex;
        justify-content: space-between;
        li {
          border-radius: 50%;
          border: 1px solid #ffffff;
          width: 1.4rem;
          height: 1.4rem;
          line-height: 1.3rem;
          margin-bottom: 0.2rem;
          &:last-of-type {
            background: orange;
            border: 1px solid orange;
          }
          &.active {
            background: #ffffff;
            border: 1px solid #ffffff;
            color: orange;
          }
        }
      }
    }
  }
}
</style>
