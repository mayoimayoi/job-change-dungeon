<script setup>
import { ref } from "vue";
import topImg from "./assets/monster1.jpg";
import monsterImg1 from "./assets/monster1.jpg";
import monsterImg2 from "./assets/monster2.jpg";
import monsterImg3 from "./assets/monster3.jpg";
import monsterImg4 from "./assets/monster4.jpg";
import monsterImg5 from "./assets/monster5.jpg";
import monsterImg6 from "./assets/monster6.jpg";

// 変数へ初期値を代入
const questionBox = [
  ["スタート"],
  [
    "社内の人間関係は良好である",
    "集中して仕事に取り組める環境が整っている",
    "社内にリフレッシュできる環境や仕組みが整っている",
    "休日はゆっくり休める",
    "仕事に行くのが楽しみ",
    "給料はいい方だと思う",
    "将来に不安はない",
    "職場に尊敬できる人がいる",
    "仕事にやりがいを感じている",
    "今の仕事は自分に合っている",
  ],
  [
    "職場に関わりたくない人がいる",
    "上司とうまくコミュニケーションが取れない",
    "給料がもっと欲しいと感じる",
    "正直ボーナスが少ない",
    "もっとやりがいのある仕事がしたい",
    "誰でもできる仕事をやっている気がする",
    "なんとなく将来が不安だ",
    "普段から残業が多い",
    "もっと休みが欲しい",
    "他の会社の仕事が気になる",
  ],
];
let questionNumber = 0;
let questionKind = 1;
let monsterNumber = 6;
const playerLife = ref("");
const monsterLife = ref("");
const quesitonContent = ref(questionBox[0][0]);
const selectBtn = ref(false);
const startBtn = ref(true);
const retryBtn = ref(false);
const lifeBox = ref(false);
const charengeResultShow = ref(false);
const keepResultShow = ref(false);
const monsterShow1 = ref(false);
const monsterShow2 = ref(false);
const monsterShow3 = ref(false);
const monsterShow4 = ref(false);
const monsterShow5 = ref(false);
const monsterShow6 = ref(false);

// 問題を20個のうちからランダム持ってくるための関数
function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min) + min); //The maximum is exclusive and the minimum is inclusive
}

// 質問をランダムで返す関数
const chnageQuestion = () => {
  questionKind = getRandomInt(1, 3);
  console.log(questionKind);
  questionNumber = getRandomInt(0, 11);
  return questionBox[questionKind][questionNumber];
};

// スタートボタンを押した時の動作
const startDiagnosis = () => {
  quesitonContent.value = chnageQuestion();
  monsterNumber = getRandomInt(1, 6);
  playerLife.value = 2;
  switch (monsterNumber) {
    case 1:
      monsterLife.value = 3;
      monsterShow1.value = !monsterShow1.value;
      break;
    case 2:
      monsterLife.value = 2;
      monsterShow2.value = !monsterShow2.value;
      break;
    case 3:
      monsterLife.value = 4;
      monsterShow3.value = !monsterShow3.value;
      break;
    case 4:
      monsterLife.value = 3;
      monsterShow4.value = !monsterShow4.value;
      break;
    case 5:
      monsterLife.value = 3;
      monsterShow5.value = !monsterShow5.value;
      break;
    case 6:
      monsterLife.value = 2;
      monsterShow6.value = !monsterShow6.value;
      break;
  }
  startBtn.value = !startBtn.value;
  selectBtn.value = !selectBtn.value;
  lifeBox.value = !lifeBox.value;
  console.log(monsterLife.value);
};

// リトライボタンを押した時の操作
const retryDiagnosis = () => {
  quesitonContent.value = questionBox[0][0];
  lifeBox.value = !lifeBox.value;
  startBtn.value = !startBtn.value;
  retryBtn.value = !retryBtn.value;
  monsterShow1.value = false;
  monsterShow2.value = false;
  monsterShow3.value = false;
  monsterShow4.value = false;
  monsterShow5.value = false;
  monsterShow6.value = false;
  charengeResultShow.value = false;
  keepResultShow.value = false;
};

// 結果を表示する
const resultDiagnosis = (resultNumber) => {
  selectBtn.value = !selectBtn.value;
  retryBtn.value = !retryBtn.value;
  if (resultNumber == 1) {
    quesitonContent.value = "あなたは今すぐ転職した方が良いです";
    charengeResultShow.value = !charengeResultShow.value;
  } else {
    quesitonContent.value = "今の職場で楽しく仕事をしよう";
    keepResultShow.value = !keepResultShow.value;
  }
};

// はいかいいえを押した時の操作
const nextQestion = (selectAnswer) => {
  if (questionKind == 1) {
    if (selectAnswer == "Yes") {
      monsterLife.value = monsterLife.value - 1;
    } else {
      playerLife.value = playerLife.value - 1;
    }
  } else {
    if (selectAnswer == "Yes") {
      playerLife.value = playerLife.value - 1;
    } else {
      monsterLife.value = monsterLife.value - 1;
    }
  }
  if (playerLife.value == 0) {
    resultDiagnosis(1);
  } else if (monsterLife.value == 0) {
    resultDiagnosis(2);
  } else {
    quesitonContent.value = chnageQuestion();
  }
};
</script>
<template>
  <header>
    <div class="">
      <img :src="topImg" alt="トップ画像" class="topimg" />
    </div>
  </header>
  <div class="container-fluid">
    <div class="mainbox row">
      <div class="col-12">
        <h1>転職ダンジョン</h1>
        <!-- モンスター画像 -->
        <div v-show="monsterShow1">
          <img :src="monsterImg1" alt="モンスター画像1" class="monsterimg" />
        </div>
        <div v-show="monsterShow2">
          <img :src="monsterImg2" alt="モンスター画像2" class="monsterimg" />
        </div>
        <div v-show="monsterShow3">
          <img :src="monsterImg3" alt="モンスター画像3" class="monsterimg" />
        </div>
        <div v-show="monsterShow4">
          <img :src="monsterImg4" alt="モンスター画像4" class="monsterimg" />
        </div>
        <div v-show="monsterShow5">
          <img :src="monsterImg5" alt="モンスター画像5" class="monsterimgg" />
        </div>
        <div v-show="monsterShow6">
          <img :src="monsterImg6" alt="モンスター画像6" class="monsterimgg" />
        </div>
        <!-- 質問画面 -->
        <div v-show="lifeBox">
          <p>モンスターのライフ</p>
          <p>{{ monsterLife }}</p>
          <p>プレイヤーのライフ</p>
          <p>{{ playerLife }}</p>
        </div>
        <div class="quesitonbox flexbox">
          <p>{{ quesitonContent }}</p>
        </div>
        <div v-show="startBtn">
          <button class="btn blue" @click="startDiagnosis">スタート</button>
        </div>
        <div v-show="selectBtn">
          <button class="btn whiteblue" @click="nextQestion('Yes')">
            はい
          </button>
          <button class="btn red" @click="nextQestion('No')">いいえ</button>
        </div>
        <!-- 結果表示画面 -->
        <div v-show="retryBtn">
          <p>転職してみよう</p>
          <div v-show="charengeResultShow">
            <a class="weblink" href="https://cpa.mynavi.jp/"
              >転職にチャレンジしてみる</a
            >
          </div>
          <div v-show="keepResultShow">
            <a class="weblink" href="https://doda.jp/engineer/"
              >やっぱり転職にチャレンジしてみる</a
            >
          </div>
          <button class="btn red w-50" @click="retryDiagnosis">リトライ</button>
        </div>
      </div>
    </div>
  </div>
  <footer class="footer mt-5 bg-light">転職応援クラブ</footer>
</template>

<style scoped>
.flexbox {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.mainbox {
  width: 90%;
  margin: 0 auto;
  padding: 20px 30px;
  background: #f1ffe0;
  box-shadow: 0px 0px 0px 10px #e2e8cc;
  border: dashed 2px white;
}
.mainbox p {
  margin: 0;
  padding: 0;
}
.quesitonbox {
  height: 200px;
  padding: 20px 30px;
  margin: 2em 0;
  background: #ffffee;
  box-shadow: 0px 0px 0px 5px #ffe00f;
  border: dashed 3px yellow;
  border-radius: 10px;
  font-size: 24px;
}
.quesitonbox p {
  margin: 0;
  padding: 0;
}
.btn {
  /* スマホ用の時はwidth: 80%;にする */
  padding: 10px 30px;
  margin: 10px 0;
  font-size: 20px;
  width: 70%;
}
.blue {
  background-color: aqua;
}
.whiteblue {
  background-color: deepskyblue;
}
.red {
  background-color: indianred;
}
.monsterimg {
  margin: 0 auto;
  width: 50%;
}
.weblink {
  font-size: 24px;
}
.topimg {
  margin: 20px auto;
  max-height: 100px;
}
</style>
