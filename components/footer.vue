<template>
  <div>
    <client-only>
      <goToTop></goToTop>
    </client-only>
    <div
      class="goup-container footer-github"
    >
      <a href="https://github.com/HelipengTony" target="_blank" style="text-decoration:none">
        <i class="ri-github-fill footer-github-icon"></i>
      </a>
    </div>
    <div class="footer reveal">
      <p>
        <i class="ri-copyright-line copy-icon"></i> Copyright 2018-{{ new Date().getFullYear() }} · TonyHe ·
        <a
          href="http://www.beian.miit.gov.cn"
          target="_blank"
          style="text-decoration: none;color: inherit;font-size: 14px;font-weight: 500;"
        >蜀ICP备16017417号-9</a> ·
        <a
          href="https://creativecommons.org/licenses/by-nc/4.0/"
          target="_blank"
          style="text-decoration: none;color: inherit;font-size: 14px;font-weight: 500;"
        >CC BY-NC 4.0</a> ·
        <a
          href="https://union.zhaodao.ai/#random"
          target="_blank"
          style="text-decoration: none;color: inherit;font-size: 14px;font-weight: 500;"
        >
        <img src="https://static.ouorz.com/makers-union.svg" class="creator-icon" /></a>
      </p>
    </div>
  </div>
</template>

<script>
import { Component, Vue } from 'nuxt-property-decorator'

@Component({})
export default class siteFooter extends Vue {}
</script>