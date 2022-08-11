<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart()">Add To Card</button>
    <div class="top-row">
      <div class="top part">
        <div class="robot-name">
          {{ selectedRobot.head.title }}
          <span v-if="selectedRobot.head.onSale" class="sale">Sale...!</span>
        </div>
        <img :src="selectedRobot.head.src" title="head"/>
        <button @click="selectPreviousHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobot.arm.src" title="left arm"/>
        <button @click="selectPreviousArm()" class="prev-selector">&#9650;</button>
        <button @click="selectedNextArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobot.torso.src" title="left arm"/>
        <button @click="selectPreviousTorsos()" class="prev-selector">&#9668;</button>
        <button @click="selectNextTorsos()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobot.arm.src" title="left arm"/>
        <button @click="selectPreviousArm()" class="prev-selector">&#9650;</button>
        <button @click="selectedNextArm()" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobot.base.src" title="left arm"/>
        <button @click="selectPreviousBase()" class="prev-selector">&#9668;</button>
        <button @click="selectNextBase()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
        <tr>
          <th>Robot</th>
          <th class="cost">Cost</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(robot, index) in cart" :key="index">
          <th>{{robot.head.title}}</th>
          <th class="cost">{{robot.head.cost}}</th>
        </tr>
        <tr style="color: red" v-if="!cart.length">Empty Cart</tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import availableParts from "../data/part";

function getPreviousValidIndex(index = 0, length = 0) {
  const deprecatedIndex = index - 1;
  return deprecatedIndex < 0 ? length - 1 : deprecatedIndex;
}

function getNextValidIndex(index = 0, length = 0) {
  const incrementedIndex = index + 1;
  return incrementedIndex > length - 1 ? 0 : incrementedIndex;
}

export default {
  name: "RobotBuilderPage",
  data() {
    return {
      cart: [],
      availableParts,
      headIndex: 0,
      armIndex: 0,
      torsosIndex: 0,
      baseIndex: 0
    };
  },

  computed: {
    selectedRobot() {
      return {
        head: this.availableParts.heads[this.headIndex],
        arm: this.availableParts.arms[this.armIndex],
        torso: this.availableParts.torsos[this.torsosIndex],
        base: this.availableParts.bases[this.baseIndex]
      };
    }
  },

  methods: {
    addToCart() {
      const cost =
          this.selectedRobot.head.cost +
          this.selectedRobot.arm.cost +
          this.selectedRobot.torso.cost +
          this.selectedRobot.base;
      // this.cart.push(Object.assign({}, this.selectedRobot, { cost }));
      this.cart.push({...this.selectedRobot, cost});
    },

    selectNextHead() {
      this.headIndex = getNextValidIndex(
          this.headIndex,
          this.availableParts.heads.length
      );
    },

    selectPreviousHead() {
      this.headIndex = getPreviousValidIndex(
          this.headIndex,
          this.availableParts.heads.length
      );
    },

    selectedNextArm() {
      this.armIndex = getNextValidIndex(
          this.armIndex,
          this.availableParts.arms.length
      );
    },
    selectPreviousArm() {
      this.armIndex = getPreviousValidIndex(
          this.armIndex,
          this.availableParts.arms.length
      );
    },
    selectNextTorsos() {
      this.torsosIndex = getNextValidIndex(
          this.torsosIndex,
          this.availableParts.torsos.length
      );
    },
    selectPreviousTorsos() {
      this.torsosIndex = getPreviousValidIndex(
          this.torsosIndex,
          this.availableParts.torsos.length
      );
    },
    selectNextBase() {
      this.baseIndex = getNextValidIndex(
          this.baseIndex,
          this.availableParts.bases.length
      );
    },
    selectPreviousBase() {
      this.baseIndex = getPreviousValidIndex(
          this.baseIndex,
          this.availableParts.bases.length
      );
    }
  }
};
</script>

<style scoped>
.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
}

.part img {
  width: 165px;
}

.top-row {
  display: flex;
  justify-content: space-around;
}

.middle-row {
  display: flex;
  justify-content: center;
}

.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}

.head {
  border-bottom: none;
}

.left {
  border-right: none;
}

.right {
  border-left: none;
}

.left img {
  transform: rotate(-90deg);
}

.right img {
  transform: rotate(90deg);
}

.bottom {
  border-top: none;
}

.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}

.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}

.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}

.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}

.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}

.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}

.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}

.right .next-selector {
  right: -3px;
}

.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}

.sale {
  color: red;
}

.content {
  position: relative;
}

.add-to-cart {
  position: absolute;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}
td, th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}
.cost {
  text-align: right;
}
</style>
