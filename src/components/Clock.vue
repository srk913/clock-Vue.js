<template>
  <div>
    <div class="container">
      <p class="location">{{ location }}</p>
      <p class="date">{{ year }}/{{ month }}/{{ day }}</p>
      <div class="time">
        <p class="time-item hours">{{ hours }}</p>
        <p class="time-item minutes">{{ minutes }}</p>
        <p class="time-item seconds">{{ seconds }}</p>
      </div>
    </div>
  </div>
</template>

<script>
//0埋め処理の関数zeroPadding
//数値(num)を指定桁数(digit)まで0埋め
const zeroPadding = (num, digit) => {
  //桁数分の0を文字列として生成 => 00
  //000の末尾にnumを結合
  //sliceで右から2桁切り出し
  //return (Array(digit).join("0") + num).slice(-digit);
  //------------------------------------------------------
  //padStartメソッド = 指定した長さに文字列を延長するメソッド
  //numを指定した桁数（ここでは全部2）にする。足りないときは"0"で埋める。
  return String(num).padStart(digit, "0");
};

export default {
  props: ["location", "diff"], // ←追加
  data() {
    return {
      date: new Date()
    };
  },
  //computed = 計算とか処理を行って、このデータで定義したデータとして扱うことができる。
  computed: {
    year() {
      return this.date.getFullYear();//年...4桁の数値を返す。
    },
    month() {
      return zeroPadding(this.date.getMonth() + 1, 2);//0が入るの、0月はないので +1 してあげる。
    },
    day() {
      return zeroPadding(this.date.getDate(), 2);
    },
    hours() {
      return zeroPadding(this.date.getHours(), 2);
    },
    minutes() {
      return zeroPadding(this.date.getMinutes(), 2);
    },
    seconds() {
      return zeroPadding(this.date.getSeconds(), 2);
    }
  },
  mounted() {
    this.setDate();
    setInterval(() => this.setDate(), 1000);
  },
  methods: {
    setDate() {
      this.date = new Date();
      this.date.setHours(this.date.getHours() + this.diff)
    }
  }
};
</script>

<style scoped>
.container {
  background-color: midnightblue;
  padding: 10px;/*コンテナの内側に10px追加*/
}
.location {
  color: white;
  font-family: "Teko", sans-serif;
  font-size: 5rem;
  letter-spacing: 0.05em;/**/
  line-height: 1;
}
.date {
  text-align: right;/*日付の右寄せ*/
  color: white;
  font-family: "Teko", sans-serif;
  font-size: 35px;
  letter-spacing: 0.1em;/*日付の字間 em で指定する。1em = 「文字の高さ」*/
  /* margin: 0em 0; */
  line-height: 1;/*日付の上下の隙間を消す*/
}
.time {
  display: flex;/*緑の箱を横並びに*/
}
.time-item {
  display: flex;
  justify-content: center;/* 数字の横位置を真ん中に */
  align-items: center;/*数字の高さを真ん中に*/
  flex: auto;/*item の横幅を調整*/
  height: 100px;
  position: relative;
  z-index: 1;
  padding: 0.5em;
  margin: 3px;
  color: #fff;
  font-family: "Roboto Mono", monospace;
  font-size: 3rem;
  line-height: 1;
  background-color: lightseagreen;
  box-sizing: border-box;/*paddingを使っているのでそこを含める*/
}
.time-item:before {/*Hours Minutes Seconds　の css*/
  position: absolute;
  right: 5px;
  bottom: 1px;
  z-index: 1;
  color: #3a4a5e;
  font-family: "Teko", sans-serif;
  font-size: 1.4rem;
  letter-spacing: 0.05em;
}
/*文字の追加*/
.hours:before {
  content: "Hours";
}
.minutes:before {
  content: "Minutes";
}
.seconds:before {
  content: "Seconds";
}
</style>