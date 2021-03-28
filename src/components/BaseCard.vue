<template>
  <div class="card">
    <div class="title">
      <slot name="title"></slot>
    </div>
    <div class="hidden">
      <slot name="hidden"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "BaseCard",
};
</script>

<style scoped>
.card {
  background: #da4242;
  width: 210px;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  height: 210px;
  border: 3px solid #1e1e1e;
  cursor: pointer;
}
.title {
  text-align: center;
  font-size: 1rem;
  color: white;
  transition: height 10ms ease-out;
}
.hidden {
  height: 0;
  overflow: hidden;
  transition: height 200ms cubic-bezier(0.165, 0.84, 0.44, 1);
  display: flex;
  flex-direction: column;
}
.hidden > * {
  color: white;
}
.card:hover .hidden {
  height: 100%;
}
.card:hover .title {
  height: 0;
  overflow: hidden;
}
</style>
