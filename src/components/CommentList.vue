<template>
  <br />
  <div class="comment-list">
    <h3 class="aaa">Comments</h3>
    <center>
      <ul>
        <li v-for="(comment, index) in comments" :key="index">
          <div class="list-style">
            <div class="index-list"></div>
            <div>{{ comment.comment }}</div>
          </div>
          <br />
        </li>
      </ul>
    </center>
  </div>
</template>
<script>
export default {
  props: {
    comments: {
      type: Array,
      required: true
    }
  }
}
</script>
<style scoped>
.list-style {
  border-radius: 5px;
  border: 1px solid black;
  background-color: white;
  height: 100px;
  width: 500px;
  text-align: center;
  margin-left: 100px;
  margin-right: 100px;
  line-height: 100px;
  height: 50px;
  width: 300px;
  border: red 3px dashed;
  line-height: 25px;
  background-color: moccasin;
  text-align: center;
}
</style>
