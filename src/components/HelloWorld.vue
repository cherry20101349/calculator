<template>
  <div class="content">
    <div class="wrapper">
      <div>{{ num === '0' ? lastNum : num }}</div>
      <ul class="row_ul">
        <li v-for="(item, index) in items" :key="index">
          <ul class="item_ul">
            <li
              v-for="(row, index) in item"
              :key="index"
              @click="onClick(row)"
            >
              {{ row.content }}
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
      num: '0',
      lastNum: 0,
      // 使用Object.freeze，vue不会对items里的object做getter、setter绑定，可优化速度，提升性能
      items: Object.freeze({
        row1: [
          { content: "AC" },
          { content: "DEL" },
          { content: "%" },
          { content: "*" },
        ],
        // row1: ['AC', 'DEL', '%', '*']
        row2: [
          { content: 7 },
          { content: 8 },
          { content: 9 },
          { content: "*" },
        ],
        row3: [
          { content: 4 },
          { content: 5 },
          { content: 6 },
          { content: "-" },
        ],
        row4: [
          { content: 1 },
          { content: 2 },
          { content: 3 },
          { content: "+" },
        ],
        row5: [{ content: 0 }, { content: "." }, { content: "=" }],
      }),
    };
  },
  methods: {
    onClick({content}) {
      console.log(content);
      if (typeof content === 'number') {
        this.num += content;
        this.num = parseFloat(this.num) + '';
      } else {
        switch (content) {
          case 'AC':
            this.num = '0';
            this.lastNum = 0;
            break;
          case 'DEL':
            this.num = this.num.slice(0, this.num.length - 1);
            this.num = !this.num ? '0' : this.num;
            break;
          case '+':
            this.lastNum = parseFloat(this.num) + parseFloat(this.lastNum);
            this.num = '0';
            break;
          // case '-':
          //   this.lastNum = parseFloat(this.lastNum) - parseFloat(this.num);
          //   this.num = '0';
          //   break;
          default:
            break;
        }
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
    li:last-of-type li:first-of-type {
      width: 3.3rem;
      border-radius: 50rem;
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
      }
    }
  }
}
</style>
