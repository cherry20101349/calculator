<template>
  <div class="content">
    <div class="wrapper">
      <div>{{ num === "0" ? lastNum : num }}</div>
      <ul class="row_ul">
        <li v-for="(item, index) in items" :key="index">
          <ul class="item_ul">
            <li
              v-for="(row, index) in item"
              :key="index"
              :class="{ active: row === activeRow }"
              @click="onClick(row)"
            >
              {{ row }}
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
      num: "0",
      lastNum: 0,
      isFirstClick: true,
      lastOperation: "",
      count: 0,
      activeRow: "",
      // 使用Object.freeze，vue不会对items里的object做getter、setter绑定，可优化速度，提升性能
      items: Object.freeze({
        row1: ["AC", "DEL", "%", "*"],
        row2: [7, 8, 9, "*"],
        row3: [4, 5, 6, "-"],
        row4: [1, 2, 3, "+"],
        row5: [0, ".", "="],
      }),
    };
  },
  methods: {
    onClick(content) {
      this.activeRow = content;
      if (typeof content === "number") {
        this.num += content;
        this.num = parseFloat(this.num) + ""; // 去掉首位数字为0
      } else {
        switch (content) {
          case "AC":
            this.empty();
            break;
          case "DEL":
            this.delete();
            break;
          case "+":
            this.add(content);
            break;
          case "-":
            this.reduce(content);
            break;
          default:
            break;
        }
      }
    },
    // 清空
    empty() {
      this.num = "0";
      this.lastNum = 0;
    },
    // 删除
    delete() {
      this.num = this.num.slice(0, this.num.length - 1);
      this.num = !this.num ? "0" : this.num;
    },
    // 加法
    add(content) {
      if (
        this.count === 0 &&
        this.lastOperation &&
        content !== this.lastOperation
      ) {
        this.operation();
      } else {
        this.count === 0;
        this.lastOperation = "+";
        this.lastNum = this.isFirstClick
          ? parseFloat(this.num)
          : parseFloat(this.num) + parseFloat(this.lastNum);
        this.isFirstClick = false;
        this.num = "0";
      }
    },
    // 减法
    reduce(content) {
      if (
        this.count === 0 &&
        this.lastOperation &&
        content !== this.lastOperation
      ) {
        this.operation();
      } else {
        this.count === 0;
        this.lastOperation = "-";
        this.lastNum = this.isFirstClick
          ? parseFloat(this.num)
          : parseFloat(this.lastNum) - parseFloat(this.num);
        this.isFirstClick = false;
        this.num = "0";
      }
    },
    operation() {
      this.count++;
      switch (this.lastOperation) {
        case "+":
          this.add(this.lastOperation);
          break;
        case "-":
          this.reduce(this.lastOperation);
          break;
        default:
          break;
      }
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
