<script setup>
import { ref } from "vue";
import topImg from "./assets/topimg.png";
import monsterImg1 from "./assets/monster1.jpg";
import monsterImg2 from "./assets/monster2.jpg";
import monsterImg3 from "./assets/monster3.jpg";
import monsterImg4 from "./assets/monster4.jpg";
import monsterImg5 from "./assets/monster5.jpg";
import monsterImg6 from "./assets/monster6.jpg";

// 変数へ初期値を代入
const questionBox = [
  ["スタートボタンを押してください"],
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
    "他の人と比べてボーナスが少ない",
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
const titleContent = ref("ランダムな質問に答えて転職モンスターを倒そう");
const quesitonContent = ref(questionBox[0][0]);
const selectBtn = ref(false);
const startBtn = ref(true);
const retryBtn = ref(false);
const lifeBox = ref(false);
const titleBox = ref(true);
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
  questionNumber = getRandomInt(0, 10);
  return questionBox[questionKind][questionNumber];
};

// スタートボタンを押した時の動作
const startDiagnosis = () => {
  quesitonContent.value = chnageQuestion();
  monsterNumber = getRandomInt(1, 7);
  showMonster(monsterNumber);
  playerLife.value = 2;
  startBtn.value = !startBtn.value;
  selectBtn.value = !selectBtn.value;
  titleBox.value = !titleBox.value;
  document.getElementById("playerLifeId").style.color = "black";
  document.getElementById("playerLifeId").style.fontWeight = "normal";
  document.getElementById("monsterLifeId").style.color = "black";
  document.getElementById("monsterLifeId").style.fontWeight = "normal";
  lifeBox.value = !lifeBox.value;
};

// リトライボタンを押した時の操作
const retryDiagnosis = () => {
  quesitonContent.value = questionBox[0][0];
  titleContent.value = "ランダムな質問に答えて転職モンスターを倒そう";
  document.getElementById("titleContentId").style.color = "black";
  document.getElementById("titleContentId").style.fontWeight = "normal";
  startBtn.value = !startBtn.value;
  retryBtn.value = !retryBtn.value;
  charengeResultShow.value = false;
  keepResultShow.value = false;
};

// 結果を表示する
const resultDiagnosis = (resultNumber) => {
  selectBtn.value = !selectBtn.value;
  retryBtn.value = !retryBtn.value;
  titleBox.value = !titleBox.value;
  lifeBox.value = !lifeBox.value;
  showMonster(monsterNumber);
  document.getElementById("titleContentId").style.color = "red";
  document.getElementById("titleContentId").style.fontWeight = "bold";
  if (resultNumber == 1) {
    titleContent.value = "転職モンスターの勝ちです";
    quesitonContent.value = "今すぐ転職して新しい一歩を踏み出そう";
    charengeResultShow.value = !charengeResultShow.value;
  } else {
    titleContent.value = "あなたの勝ちです";
    quesitonContent.value = "今の職場で楽しく仕事をしよう";
    keepResultShow.value = !keepResultShow.value;
  }
};

// はいかいいえを押した時の操作
const nextQestion = (selectAnswer) => {
  document.getElementById("playerLifeId").style.color = "black";
  document.getElementById("playerLifeId").style.fontWeight = "normal";
  document.getElementById("monsterLifeId").style.color = "black";
  document.getElementById("monsterLifeId").style.fontWeight = "normal";
  if (questionKind == 1) {
    if (selectAnswer == "Yes") {
      monsterLife.value = monsterLife.value - 1;
      document.getElementById("monsterLifeId").style.color = "red";
      document.getElementById("monsterLifeId").style.fontWeight = 900;
    } else {
      playerLife.value = playerLife.value - 1;
      document.getElementById("playerLifeId").style.color = "red";
      document.getElementById("playerLifeId").style.fontWeight = 900;
    }
  } else {
    if (selectAnswer == "Yes") {
      playerLife.value = playerLife.value - 1;
      document.getElementById("playerLifeId").style.color = "red";
      document.getElementById("playerLifeId").style.fontWeight = 900;
    } else {
      monsterLife.value = monsterLife.value - 1;
      document.getElementById("monsterLifeId").style.color = "red";
      document.getElementById("monsterLifeId").style.fontWeight = 900;
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

// 該当の番号のモンスター画像の表示を切り替える関数（非表示にもする）
const showMonster = (monsterNumber) => {
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
        <!-- モンスター画像 -->
        <div class="monsterimg">
          <div v-show="monsterShow1">
            <img :src="monsterImg1" alt="モンスター画像1" />
          </div>
          <div v-show="monsterShow2">
            <img :src="monsterImg2" alt="モンスター画像2" />
          </div>
          <div v-show="monsterShow3">
            <img :src="monsterImg3" alt="モンスター画像3" />
          </div>
          <div v-show="monsterShow4">
            <img :src="monsterImg4" alt="モンスター画像4" />
          </div>
          <div v-show="monsterShow5">
            <img :src="monsterImg5" alt="モンスター画像5" />
          </div>
          <div v-show="monsterShow6">
            <img :src="monsterImg6" alt="モンスター画像6" />
          </div>
        </div>
        <!-- 質問画面 -->
        <div v-show="lifeBox">
          <div class="row">
            <div class="col-md-2"></div>
            <div class="col-md-4 mt-3">
              <p>モンスターのライフ</p>
              <p id="monsterLifeId">{{ monsterLife }}</p>
            </div>
            <div class="col-md-4 mt-3">
              <p>あなたのライフ</p>
              <p id="playerLifeId">{{ playerLife }}</p>
            </div>
            <div class="col-md-2"></div>
          </div>
        </div>
        <div v-show="titleBox" class="mt-3">
          <h1 id="titleContentId">{{ titleContent }}</h1>
        </div>
        <div class="quesitonbox flexbox mt-3">
          <p>{{ quesitonContent }}</p>
        </div>
        <div class="mt-3">
          <div v-show="startBtn">
            <button class="startbtn blue" @click="startDiagnosis">
              スタート
            </button>
          </div>
          <div v-show="selectBtn">
            <div class="row">
              <div class="col-md-2"></div>
              <div class="col-md-4 mt-3">
                <button class="btn whiteblue" @click="nextQestion('Yes')">
                  はい
                </button>
              </div>
              <div class="col-md-4 mt-3">
                <button class="btn red" @click="nextQestion('No')">
                  いいえ
                </button>
              </div>
              <div class="col-md-2"></div>
            </div>
          </div>
        </div>
        <!-- 結果表示画面 -->
        <div v-show="retryBtn">
          <div v-show="charengeResultShow">
            <p>↓↓転職にチャレンジしてみる↓↓</p>
            <a class="weblink" href="https://cpa.mynavi.jp/"
              >転職チャレンジサイト</a
            >
          </div>
          <div v-show="keepResultShow">
            <p>↓↓やっぱりチャレンジしてみたいと思ったら↓↓</p>
            <a class="weblink" href="https://doda.jp/engineer/"
              >転職チャレンジサイト</a
            >
          </div>
          <button class="startbtn red" @click="retryDiagnosis">リトライ</button>
        </div>
      </div>
    </div>
  </div>
  <footer class="footer mt-5 bg-light">
    <p>転職応援クラブ</p>
  </footer>
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
  height: 100px;
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
  padding: 10px 20px;
  margin: 10px 0;
  font-size: 20px;
  width: 90%;
}
.startbtn {
  padding: 10px 20px;
  margin: 10px 0;
  font-size: 20px;
  width: 50%;
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
@media screen and (max-width: 639px) {
  /*スマホ用のcssを記述*/
  .monsterimg {
    width: 100%;
  }
  .btn {
    /* スマホ用の時はwidth: 80%;にする */
    width: 70%;
  }
  .quesitonbox {
    height: 70px;
    font-size: 20px;
  }
  .startbtn {
    padding: 10px 20px;
    margin: 10px 0;
    font-size: 20px;
    width: 90%;
  }
}
.weblink {
  font-size: 24px;
}
.topimg {
  margin: 20px auto;
  max-height: 70px;
}
</style>
