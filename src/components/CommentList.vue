<template>
  <ul class="comment-list">
    <h2 v-if="comments.length > 0" class="comment-list__title">Список постов</h2>
    <h2 class="comment-list__title comment-list__title--empty" v-else>Список пуст</h2>
    <transition-group name="comment-list">
      <comment-item
        @showDialog="changeParentCommentId"
        v-for="comment in reactionAndCommentNum"
        :comment="comment"
        :key="comment.comment.id"
        class="comment-list__item"
      />
    </transition-group>
  </ul>
</template>

<script>
import commentItem from "./CommentItem.vue";

export default {
  components: {
    commentItem,
  },
  props: {
    comments: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      commentList: [],
    };
  },
  computed: {
    sortByNesting() { //метод принимает список комментариев
      let sortedComments = [];
      this.commentList.forEach((comment) => {
        if (!comment.parentId) { // Если parentId нет, значит это комментарий на верхнем уровне
          sortedComments.push({
            comment: comment,
            nest: 0, //уровень вложенности
            reactionSum: 0,
            children: 0,
          });
          this.haveChild(comment, 1, this.commentList, sortedComments);
        }
      });
      return sortedComments;
    },
    reactionAndCommentNum() { //счет реакций
      let arr = this.sortByNesting;
      for (let i = 0; i < arr.length; i++) {
        for (let j = i + 1; j < arr.length; j++) {
          const parent = arr[i];
          const child = arr[j]; 
          if (parent.comment.id === child.comment.parentId) {
            parent.children++;
            parent.reactionSum += child.comment.reaction;
          }
        }
      }
      return arr;
    },
  },
  methods: {
    haveChild(parent, nest, arr1, arr2) {
      arr1.forEach((element) => {
        if (parent.id === element.parentId) { //Если id родительского комментария совпадает с parentId текущего элемента, то этот элемент является дочерним комментарием.
          arr2.push({
            comment: element,
            nest: nest,
            reactionSum: 0,
            children: 0,
          });
          this.haveChild(element, nest + 1, arr1, arr2);
        }
      });
    },
    changeParentCommentId(parentCommentId) {
      this.$emit("reply", parentCommentId);
    },
  },
  watch: {
    comments: {
      handler(newComments) {
        this.commentList = newComments;
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
.empty-message {
  color: #ff0000;
}

.comment-list {
  margin-top: 30px;
  padding-left: 0;
  list-style: none;
}

.comment-list__title {
  font-size: 28px;
  margin-bottom: 15px;
  color: #333;
}

.comment-list-enter-active,
.comment-list-leave-active {
  transition: opacity 1s ease, transform 1s ease;
}

.comment-list-enter-from,
.comment-list-leave-to {
  opacity: 0;
  transform: translateX(200px);
}

.comment-list__title--empty {
  color: #ff0000;
}
</style>
