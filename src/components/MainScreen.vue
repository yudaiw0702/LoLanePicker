<template>
  <div>
    <ol>
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

      <span class="Samoner">サモナーネームを入力：</span>
      <input v-model="item" />
      <button v-bind:disabled="isPush" v-on:click="add_item()">追加</button>
    </ol>

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
    <span>Checked names: {{ checkedNoraLane }}</span>

    <li v-for="(it, index) in items" :key="index">
      {{ it }}
      <button v-on:click="del_item(index)">x</button>
    </li>
    <div>
      <button v-on:click="lane_pick()">レーンを決める</button>
      <div>
        <li v-for="(id, index) in lane" :key="index">
          {{ id.lane }}：{{ id.name }}
        </li>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoLanePicker",
  props: {
    msg: String,
  },
  data: function () {
    return {
      selectedPlayers: "",
      optionNumbers: [{ id: 1 }, { id: 2 }, { id: 3 }, { id: 4 }, { id: 5 }],
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
      if (this.items.length < this.selectedPlayers) {
        this.items.push(this.item);
        if (this.items.length > this.selectedPlayers - 1) this.isPush = true;
      }
    },
    del_item: function (_index) {
      this.items.splice(_index, 1);
    },
    lane_pick: function () {
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
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
