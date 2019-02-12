<template>
	<div class="hello">
		<div class="hello-inner">
			<h1 class="title"> 李漂亮选名大会 </h1>
      <el-button @click="reset">重置</el-button>
      <el-checkbox-group v-model="chosenSeconds" class="checkbox-container">
        <el-checkbox-button v-for="second in seconds" :key="second" :label="second" border></el-checkbox-button>
      </el-checkbox-group>
      <el-checkbox-group v-model="chosenThirds" class="checkbox-container">
        <el-checkbox-button v-for="third in thirds" :key="third" :label="third"></el-checkbox-button>
      </el-checkbox-group>
		</div>
    <div class="result-container" v-if="tempResult && tempResult.length">
      <h2 class="title">嗯，就从这里选吧（{{tempResult.length}}个）</h2>
       <el-tag v-for="name in tempResult"
               type="success"
               :key="name"
               class="result-item">{{name}}</el-tag>

    </div>
	</div>
</template>

<script>
    const FIRST = '李';

    const SECONDS = [
      '府', '宙', '秉', '舍', '承',
      '岸', '玖', '松', '宗', '昂',
      '岱', '附', '庚', '沅', '昀',
      '易', '妮', '其', '姗', '宛',
      '汶', '沂', '宜'
    ];

    const THIRDS = [
      '偿', '鸿', '徽', '键', '鞠',
      '骏', '励', '联', '沣', '与',
      '蔓', '谦', '蔚', '泽', '斋',
    ];

    export default {
      name: 'NameChoice',
      data() {
        return {
          first: FIRST,
          seconds: SECONDS,
          thirds: THIRDS,
          chosenSeconds: [],
          chosenThirds: [],
        }
      },
      mounted() {
        const chosenSeconds = window.localStorage.getItem('chosenSeconds');
        const chosenThirds = window.localStorage.getItem('chosenThirds');

        if (chosenSeconds && chosenThirds) {
          this.chosenSeconds = chosenSeconds.split('@');
          this.chosenThirds = chosenThirds.split('@');
        }
      },
      methods: {
        getAll(second, third) {
          let result = [];
          for (let i = 0; i < second.length; i++) {
            for (let j = 0; j < third.length; j++) {
              result.push(`${FIRST}${second[i]}${third[j]}`)
            }
          }
          return result;
        },
        reset () {
          this.chosenSeconds = [];
          this.chosenThirds = [];
          window.localStorage.clear()
        }
      },
      computed: {
        tempResult() {
          if (this.chosenSeconds.length && this.chosenThirds.length) {
            window.localStorage.setItem('chosenSeconds', this.chosenSeconds.join('@'));
            window.localStorage.setItem('chosenThirds', this.chosenThirds.join('@'));
            return this.getAll(this.chosenSeconds, this.chosenThirds)
          }
        }
      }
    }
</script>r5

<style>
	* {
    margin: 0;
    padding: 0;
  }
  html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    display: flex;
    width: 100%;
    min-height: 100%;
  }
  .hello-inner {
    text-align: center;
  }
  .title {
    text-align: center;
    margin: 20px 0;
  }
  .checkbox-container, .result-container {
    margin: 30px auto;
    text-align: center;
  }
  .result-item {
    margin: 5px
  }
  .el-checkbox-button .el-checkbox-button__inner,
  .el-checkbox-button:first-child .el-checkbox-button__inner,
  .el-checkbox-button.is-checked .el-checkbox-button__inner {
    border-left: 1px solid #dcdfe6;
    border-radius: 4px;
  }
</style>
