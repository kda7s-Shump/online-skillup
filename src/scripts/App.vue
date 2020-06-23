<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.9.1/underscore-min.js"></script>

<template>
  <div>
      <button @click="onShuffleClick">shuffle</button>
      <button @click="onAddButtonClick">add</button>
      <br>
      <ul class="list">
        <transition-group name="flip">
          <template v-for="item in $data.list">
            <li :key="item" class="item">
              <div class="item__text">{{ item }}</div>
              <div class="item__delete" @click="onDeleteClick(item)"></div>
            </li>
          </template>
        </transition-group>
      </ul>
    </div>

</template>

<script>
import Vue from 'vue';
import socket from './utils/socket';

export default Vue.extend({
  data() {
    return {
      list: [1, 2, 3, 4, 5]
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!');
    });

    socket.on('send', (message) => {
      console.log(message);
      this.$data.message = message;
    });
  },
  methods: {
    /**
     * シャッフルボタンをクリックした時
     */
    onShuffleClick() {
      this.$data.list = _.shuffle(this.$data.list);
    },
    /**
     * 追加ボタンをクリックした時
     */
    onAddButtonClick() {
      const value = Math.max(...this.$data.list) + 1;
      const index = Math.floor(this.$data.list.length * Math.random());
      this.$data.list.splice(index, 0, value);
    },
    /**
     * 各要素のバツボタンをクリックした時
     * @param {number} item - 要素の番号
     */
    onDeleteClick(item) {
      const index = this.$data.list.indexOf(item);
      this.$data.list.splice(index, 1);
    }
  }
});
</script>

<style lang="scss" scoped>
.list {
  position: relative;
  margin: 20px 0 0;
  padding: 0;
}

.item {
  position: relative;
  display: inline-block;
  padding: 10px 20px;
  transition: all 0.5s;

  &__delete {
    position: absolute;
    top: 0;
    right: 0;
    width: 20px;
    height: 20px;

    &::before, &::after {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 70%;
      height: 2px;
      background-color: #ccc;
      content: '';
    }

    &::before {
      transform: translate3d(-50%, -50%, 0) rotate(45deg);
    }

    &::after {
      transform: translate3d(-50%, -50%, 0) rotate(135deg);
    }
  }
}

.flip {
  // 要素が動くときにtransitionを設定する（.itemでtransitionを設定しているため-moveで書く必要はない）
  // &-move {
  //   transition: transform 0.5s;
  // }

  // 要素が入るときのアニメーション
  &-enter {
    &-active {
      opacity: 0;
      transform: translate3d(0, -30px, 0);
    }

    &-to {
      opacity: 1;
      transform: translate3d(0, 0, 0);
    }
  }

  // 要素が消える時のアニメーション
  &-leave {
    &-active {
      position: absolute;
    }

    &-to {
      opacity: 0;
      transform: translate3d(0, -30px, 0);
    }
  }
}
</style>
