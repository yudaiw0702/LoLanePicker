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
      <input v-model="item" />
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

    <li v-for="(item, index) in itemsBef" :key="index">
      {{ item }}
      <button v-on:click="del_item(index)">x</button>
    </li>
    <div>
      <button v-on:click="lane_pick()">レーンを決める</button>
      <div class="box1">
        <ul id="nav">
          <div>
            <p class="top">トップ：{{ lane[0].name }}</p>
            <p class="jungle">ジャングル：{{ lane[1].name }}</p>
            <p class="mid">ミッド：{{ lane[2].name }}</p>
            <p class="support">サポート：{{ lane[3].name }}</p>
            <p class="adc">ボット：{{ lane[4].name }}</p>
          </div>
        </ul>
      </div>
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

  methods: {
    add_item: function () {
      console.log(this.items.length);
      console.log(this.selectedPlayers);
      if (this.itemsBef.length < this.selectedPlayers) {
        this.itemsBef.push(this.item);
        //野良の人数分のチェックしたかの確認
        if (this.itemsBef.length > this.selectedPlayers - 1) this.isPush = true;
      }
      console.log(this.itemsBef);
    },
    del_item: function (_index) {
      this.itemsBef.splice(_index, 1);
    },
    lane_pick: function () {
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
      console.log(this.items);

      for (let i = 0; i < 5; i++) {
        if (this.lane[i].name == null && this.items.length >= this.arrCount) {
          this.lane[i].name = this.items[this.arrCount];
          this.arrCount++;
        }
      }
      console.log("itemsBef:" + this.itemsBef);
    },
    canCheck: function () {
      if (this.selectedPlayers == 5) {
        this.isChecked = true;
      } else if (5 - this.selectedPlayers == 1 + this.checkedNoraLane.length) {
        this.isChecked = true;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box1 {
  padding: 0.5em 1em 1em;
  margin: 2em 0;
  font-weight: bold;
  border: solid 3px #000000;
}
.box1 p {
  margin: 0;
  padding: 20;
}
.size {
  padding: 0.5em 10em 0.5em;
}

.top {
  padding-left: 1px;
  line-height: 50px;
  background: url(../assets/TOP.png) no-repeat;
  background-size: 3%;
}

.jungle {
  padding-left: 50px;
  line-height: 50px;
  background: url(../assets/JUNGLE.png) no-repeat;
  background-size: 3%;
}

.mid {
  padding-left: 50px;
  line-height: 50px;
  background: url(../assets/MIDDLE.png) no-repeat;
  background-size: 3%;
}

.support {
  padding-left: 50px;
  line-height: 50px;
  background: url(../assets/SUPPORT.png) no-repeat;
  background-size: 3%;
}

.adc {
  padding-left: 50px;
  line-height: 50px;
  background: url(../assets/ADC.png) no-repeat;
  background-size: 3%;
}
</style>
