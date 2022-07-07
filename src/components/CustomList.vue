<template>
  <div class="wrap">
    <div class="title">
      <slot name="title" :titleSlot="titleCtd">书籍列表默认标题</slot>
    </div>
    <hr />
    <div class="content">
      <div class="content-item" 
          draggable="true"
          v-for="(item, index) in books" :key="Date.now() + '-' + index" 
          @dragenter="dragenter($event, index)"
          @dragstart="dragstart(index)"
          @dragend="dragend(index)"
        >
        <slot name="item" :iconSlot="item.icon" :nameSlot="`【${item.name}】`"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    //标题
    title: {
      type: String,
      default: ''
    },
    //书籍列表
    books: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {
      dragIndex:0
    };
  },
  computed: {
    titleCtd() {
      return `【${this.title}】`;
    }
  },
  methods:{
  // 拖拽开始
  dragstart(index) {
    this.dragIndex = index;
  },
  dragenter(e, index) {
    if (this.dragIndex !== index) {
      const source = this.books[this.dragIndex];
      this.books.splice(this.dragIndex, 1);
      this.books.splice(index, 0, source);
      this.dragIndex = index;
    }
  },
  // 拖拽完毕
  dragend(index) {
    console.log('index',index)
    this.$emit('draggable',this.dragIndex)
  }
  }
};
</script>

<style lang="scss" scoped>
.wrap {
  padding: 30px 20px 30px 60px;
  background-color: #eee;

  .title {
    padding-bottom: 10px;
    color: blue;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
  }
  .content {
    padding-top: 10px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;
    > .content-item {
      padding-bottom: 10px;
      display: flex;
      flex-direction: row;
      justify-content: flex-start;
      align-items: center;
      cursor: pointer;
    }

    .content-item:last-child {
      padding-bottom: 0;
    }
  }
}
</style>
