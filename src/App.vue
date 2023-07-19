<script setup lang="ts">
import { Ref, ref } from "vue";

type DataItem = {
  name: string;
  value: string;
};

const generateData = (): DataItem[] => {
  const data = [];
  for (let i = 0; i < 30; i++) {
    data.push({
      name: "foo" + i,
      value: "bar" + i,
    });
  }
  return data;
};

const activeDataId: Ref<number> = ref(0);
const value: Ref<string> = ref("");
const data: Ref<DataItem[]> = ref(generateData());

const onClickForm = () => {
  data.value[activeDataId.value].value = value.value; // как много велью
};
</script>

<template>
  <div class="lay">
    <div class="labels">
      <form @submit.prevent>
        <label>
          <div>Value</div>
          <input
            :value="value"
            @input="(event) => value = (event?.target as HTMLInputElement).value"
            type="text"
          />
        </label>

        <button @click="onClickForm" class="submit-btn">updateValue!</button>
      </form>
    </div>
    <div class="result">
      <div
        class="item"
        v-for="(item, index) in data"
        @click="() => (activeDataId = index)"
        :class="{ active: index === activeDataId }"
      >
        <div>
          {{ item.name }}
        </div>
        <div>
          {{ item.value }}
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.lay {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #0b0736 0%, #603a66 100%);

  .result {
    max-width: 400px;
    width: 100%;
    height: 200px;
    overflow-y: auto;
    overflow-x: hidden;
    position: relative;
    .item {
      cursor: pointer;
      &.active {
        background: green;
      }
    }
    &::-webkit-scrollbar {
      width: 10px;
    }
    &::-webkit-scrollbar-track {
      background-color: rgba(175, 169, 182, 0.5);
    }
    &::-webkit-scrollbar-thumb {
      box-shadow: inset 0 0 6px rgba(175, 169, 182, 0.3);
    }

    .item {
      padding: 2px;
      border: 1px solid rgb(175, 169, 182);
      color: rgb(175, 169, 182);
      &:not(&:first-child) {
        margin-block-start: 1px;
      }
    }
  }

  .labels {
    // margin-block-start: 25%;
    max-width: 400px;
    width: 100%;
    padding: 20px;
    flex-grow: 0;
    flex-shrink: 1;

    form {
      margin-block-start: 40px;
      label {
        font-size: 15px;
        color: rgb(175, 169, 182);
        display: block;
        &:not(:first-child) {
          margin-block-start: 20px;
        }
        input {
          margin-block-start: 10px;
          padding: 10px 10px 10px 15px;
          border-radius: 25px;
          box-shadow: inset 0 0 0 50px rgb(97, 94, 100);
          -webkit-text-fill-color: #fff;
        }
      }

      .submit-btn {
        background: rgb(202, 202, 0);
        border-radius: 20px;
        padding: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        margin-block-start: 15px;
        color: #ffffff;
        font-weight: 700;
        transition: 0.3s;
        cursor: pointer;
        &:hover {
          background: rgb(146, 146, 2);
        }
        &.disabled {
          pointer-events: none;
          opacity: 0.7;
        }
      }
    }

    .checkbox-wrap {
      margin-block-start: 15px;
      display: flex;
      // justify-content: center;
      align-items: center;
      .fake-checkbox {
        width: 20px;
        height: 20px;
        background: rgb(202, 202, 0);
        border-radius: 5px;
        cursor: pointer;
        margin-inline-end: 10px;
        position: relative;
        overflow: hidden;

        &::after,
        &::before {
          opacity: 0;
          transition: 0.3s;
          content: "";
        }

        &.active {
          &::before {
            position: absolute;
            opacity: 1;
            left: 0;
            top: 50%;
            height: 30%;
            width: 3px;
            background-color: #000000;

            transform: translateX(6px) rotate(-45deg);
            transform-origin: left bottom;
          }
          &::after {
            position: absolute;
            opacity: 1;
            left: 0;
            bottom: 15%;
            height: 3px;
            width: 80%;
            background-color: #000000;

            transform: translateX(8px) rotate(-45deg);
            transform-origin: left bottom;
          }
        }
      }

      span {
        line-height: 0;
        font-size: 12px;
        color: #ffffff;
      }
    }

    .line {
      width: 80%;
      height: 2px;
      background: rgb(175, 169, 182);
      margin-block-start: 20px;
      position: relative;
      left: 50%;
      transform: translateX(-50%);
    }

    .forgot {
      margin-block-start: 15px;
      text-align: center;
      position: relative;
      left: 50%;
      transform: translateX(-50%);
      cursor: pointer;
      color: rgb(175, 169, 182);
    }
  }
}
</style>
