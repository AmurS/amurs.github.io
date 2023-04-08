<template>
  <footer>
    Powered by 
    <a href="https://github.com/vuejs/vitepress" target="_blank" title="Vitepress"
      >VitePress</a>
    | Theme by
    <a href="https://github.com/clark-cui/vitepress-blog-zaun" target="_blank" title="Clark-Cui-blog-zaun"
      >blog-zaun</a>
    | <a href="https://up.korone.my.id" target="_blank" title="KoroNetwork Server Status"
      >Server Status</a>
    <br> rkgk © 2022-{{ date }}
  </footer>
</template>
<script lang="ts" setup>
const date = new Date().getFullYear();
</script>
<style scoped>
footer {
  text-align: center;
  position: absolute;
  width: 100%;
  bottom: 20px;
  padding: 0 20px;
  background: var(--vp-c-bg);
  color: var(--vp-c-text);
}
a {
  color: var(--vp-c-brand);
  font-weight: bold;
}
a:hover {
  text-decoration: none;
  color: var(--vp-c-hover);
}
</style>
