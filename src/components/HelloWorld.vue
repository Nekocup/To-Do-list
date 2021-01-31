<template>
  <div>
    <div class="sm:w-2/3 lg:w-1/2 bg-yellow-100 mx-auto rounded-lg whiteboard">
      <div
        class="title flex items-center justify-center border-solid border-b-4 border-light-blue-500"
      >
        <p class="text-3xl">To-Do List</p>
      </div>
      <div class="content">
        <nav class="flex justify-center py-1">
          <button
            class="text-2xl w-1/3 lg:w-1/4 bg-yellow-400 m-3 p-0.5 rounded-2xl hover:bg-yellow-300 focus:outline-none"
            @click="pickAll"
          >
            Pick All
          </button>
          <button
            class="text-2xl w-1/3 lg:w-1/4 bg-yellow-400 m-3 p-0.5 rounded-2xl hover:bg-yellow-300 focus:outline-none"
            @click="deleteDone()"
          >
            DELETE DONE
          </button>
          <button
            class="text-2xl w-1/3 lg:w-1/4 bg-yellow-400 m-3 p-0.5 rounded-2xl hover:bg-yellow-300 focus:outline-none"
            @click="deletePick()"
          >
            DELETE PICK
          </button>
        </nav>
        <input
          type="text"
          id="work"
          class="bg-transparent border-dashed border-b-4 border-blue-400 focus:outline-none"
          v-model="inputWork"
          @compositionstart="handleInputStart"
          @compositionend="handleInputEnd"
          @keydown.enter="handleInputWork"
        />
        <button
          class="mx-3 rounded-full bg-white p-2 border-solid border-gray-400 border-2 focus:outline-none"
          @click="handleInputWork"
        >
          Submit
        </button>
        <div class="container p-5">
          <div class="w-full flex justify-center">
            <p class="text-lg sm:text-2xl w-1/6">Pick</p>
            <p class="text-lg sm:text-2xl w-1/6">Done</p>
            <p class="text-lg sm:text-2xl w-3/6">Work Name</p>
            <p class="text-lg sm:text-2xl w-1/6">DELETE</p>
          </div>
          <div class="flex" v-for="item in list" :key="item.id">
            <div class="w-1/6">
              <input
                type="checkbox"
                :checked="item.pick"
                @click="handlePick(item.id)"
              />
            </div>
            <div class="w-1/6">
              <input
                type="checkbox"
                :checked="item.done"
                @click="handleDone(item.id)"
              />
            </div>
            <div class="w-3/6 break-words">
              <p class="text-lg sm:text-2xl">{{ item.text }}</p>
            </div>
            <div class="w-1/6">
              <button @click="removeWork(item.id)">---</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputWork: "",
      list: [],
      compositionStatus: false,
      id: 0,
    };
  },
  methods: {
    handlePick(id) {
      this.list.forEach((data) => {
        if (data.id === id) {
          data.pick = !data.pick;
        }
      });
    },
    handleDone(id) {
      this.list.forEach((data) => {
        if (data.id === id) {
          data.done = !data.done;
        }
      });
    },
    removeWork(id) {
      this.list = this.list.filter((data) => {
        return data.id !== id;
      });
    },
    handleInputWork() {
      if (this.compositionStatus) {
        return;
      }
      if (this.inputWork === "") {
        alert("不能為空值");
        return;
      }
      this.list.push({
        id: this.id,
        text: this.inputWork,
        pick: false,
        done: false,
      });
      this.id++;
      this.inputWork = "";
    },
    handleInputStart() {
      this.compositionStatus = true;
    },
    handleInputEnd() {
      this.compositionStatus = false;
    },
    pickAll() {
      this.list.forEach((data) => {
        data.pick = true;
      });
    },
    deleteDone() {
      this.list = this.list.filter((data) => {
        return !data.done;
      });
    },
    deletePick() {
      this.list = this.list.filter((data) => {
        return !data.pick;
      });
    },
  },
};
</script>

<style lang="scss" scoped>
@font-face {
  font-family: Indie;
  src: url("../assets/fontStyle/IndieFlower-Regular.ttf");
}

.whiteboard {
  min-height: 600px;
}

.title {
  height: 150px;
}

div {
  font-family: Indie;
}
</style>