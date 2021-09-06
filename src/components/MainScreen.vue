<template>
  <div class="size">
    <div class="box1">
      <div>
        <span>何人でやる？：</span>
        <select v-model="selectedPlayers">
          <option
            v-for="number in optionNumbers"
            v-bind:value="number.id"
            v-bind:key="number.id"
          >
            {{ number.id }}にん
          </option>
        </select>
      </div>

      <span class="Samoner">サモナーネームを入力：</span>
      <input id="form1" v-model="item" />
      <button v-bind:disabled="isPush" v-on:click="add_item()">追加</button>
    </div>
    <div class="box1">
      <div>
        野良がやるレーンにチェックいれてくれ：残り{{ 5 - selectedPlayers }}人
      </div>

      <input
        type="checkbox"
        id="top"
        value="トップ"
        v-model="checkedNoraLane"
        v-on:click="canCheck()"
        :disabled="isChecked"
      />
      <label for="top">トップ</label>

      <input
        type="checkbox"
        id="jungle"
        value="ジャングル"
        v-model="checkedNoraLane"
        v-on:click="canCheck()"
        :disabled="isChecked"
      />
      <label for="jungle">ジャングル</label>
      <input
        type="checkbox"
        id="mid"
        value="ミッド"
        v-model="checkedNoraLane"
        v-on:click="canCheck()"
        :disabled="isChecked"
      />
      <label for="mid">ミッド</label>
      <input
        type="checkbox"
        id="support"
        value="サポート"
        v-model="checkedNoraLane"
        v-on:click="canCheck()"
        :disabled="isChecked"
      />
      <label for="support">サポート</label>
      <input
        type="checkbox"
        id="adc"
        value="ボット"
        v-model="checkedNoraLane"
        v-on:click="canCheck()"
        :disabled="isChecked"
      />
      <label for="adc">ボット</label>
      <br />
    </div>

    <div class="box1">
      <li v-for="(item, index) in itemsBef" :key="index">
        {{ item }}
        <button v-on:click="del_item(index)">x</button>
      </li>

      <button v-on:click="lane_pick()">レーンを決める</button>
      <button v-on:click="clean_data()">サモナー以外リセット</button>
    </div>

    <div class="box2">
      <ul id="nav">
        <div>
          <div class="text">
            <img class="top" src="../assets/TOP.png" />
            トップ：{{ lane[0].name }}
          </div>
          <div class="text">
            <img class="jungle" src="../assets/JUNGLE.png" />
            ジャングル：{{ lane[1].name }}
          </div>
          <div class="text">
            <img class="middle" src="../assets/MIDDLE.png" />
            ミッド：{{ lane[2].name }}
          </div>
          <div class="text">
            <img class="support" src="../assets/SUPPORT.png" />
            サポート：{{ lane[3].name }}
          </div>
          <div class="text">
            <img class="adc" src="../assets/ADC.png" />
            ボット：{{ lane[4].name }}
          </div>
        </div>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoLanePicker",
  data: function () {
    return {
      selectedPlayers: "",
      optionNumbers: [{ id: 1 }, { id: 2 }, { id: 3 }, { id: 4 }, { id: 5 }],
      itemsBef: [],
      items: [],
      item: "",
      isPush: false,
      lane: [
        { lane: "トップ", name: null },
        { lane: "ジャングル", name: null },
        { lane: "ミッド", name: null },
        { lane: "サポート", name: null },
        { lane: "ボット", name: null },
      ],
      checkedNoraLane: [],
      isChecked: false,
      arrCount: 0,
    };
  },

  watch: {
    selectedPlayers: function () {
      //プレイヤー人数が前と大きくなったときと少なくなったときの動作
      if ((this.itemsBef.length <= this, this.selectedPlayers)) {
        this.isPush = false;
        this.clean_data();
      } else {
        this.isPush = false;
        this.items = [];
        this.itemsBef = [];
        this.clean_data();
      }
    },
  },

  methods: {
    add_item: function () {
      if (this.itemsBef.length < this.selectedPlayers) {
        this.itemsBef.push(this.item);
        //野良の人数分のチェックしたかの確認
        if (this.itemsBef.length > this.selectedPlayers - 1) this.isPush = true;
      }

      this.item = []; //値追加後に入力欄を空にする
    },
    del_item: function (_index) {
      this.itemsBef.splice(_index, 1);
    },
    lane_pick: function () {
      //itemsにitemsBefの配列をコピー
      this.items = this.itemsBef.slice();

      //Randomにitem配列の順番を変える処理
      for (let i = this.items.length - 1; i > 0; i--) {
        let r = Math.floor(Math.random() * (i + 1));
        let tmp = this.items[i];
        this.items[i] = this.items[r];
        this.items[r] = tmp;
      }

      for (let i = 0; i < this.checkedNoraLane.length; i++) {
        for (let b = 0; b < this.lane.length; b++) {
          if (this.checkedNoraLane[i] == this.lane[b].lane) {
            this.lane[b].name = "野良" + [i + 1];
          }
        }
      }

      for (let i = 0; i < 5; i++) {
        if (this.lane[i].name == null && this.items.length >= this.arrCount) {
          this.lane[i].name = this.items[this.arrCount];
          this.arrCount++;
        }
      }
      this.arrCount = 0;
    },
    canCheck: function () {
      if (this.selectedPlayers == 5) {
        this.isChecked = true;
      } else if (5 - this.selectedPlayers == 1 + this.checkedNoraLane.length) {
        this.isChecked = true;
      }
    },
    clean_data: function () {
      //野良が行くレーンと振り当てられたレーンの配列を消す
      this.checkedNoraLane = [];
      for (let i = 0; i < 5; i++) {
        this.lane[i].name = null;
      }

      //チェックできるようにする
      this.isChecked = false;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box1 {
  text-align: center;
  padding: 10px 10px 10px;
  margin: 10px 10px 10px;
  font-weight: bold;
  border: solid 3px #000000;
  font-size: 20px;
}
.box1 p {
  margin: 0;
  padding: 20;
}
.box2 {
  padding: 10px 10px 10px;
  margin: 10px 10px 10px;
  font-weight: bold;
  border: solid 3px #000000;
}
.size {
  padding: 10px 0px 0.5em;
  margin: 10px 600px 20px;
}

.text {
  padding-right: 2em;
  padding-left: 2em;
  text-align: left;
  font-size: 40px;
}

.top {
  vertical-align: middle;
  padding-right: 80px;
  width: 90px;
}

.jungle {
  vertical-align: middle;
  width: 90px;
}

.middle {
  vertical-align: middle;
  padding-right: 80px;
  width: 90px;
}

.support {
  vertical-align: middle;
  padding-right: 40px;
  width: 90px;
}

.adc {
  vertical-align: middle;
  padding-right: 80px;
  width: 90px;
}
</style>
