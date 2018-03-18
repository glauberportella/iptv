<template>
  <footer class="page-footer">
    <div class="footer-copyright">
      <div class="container">
        Powered by {{ sponsor }} © 2018
        Ver.{{ version }}@{{ built.toLocaleString() }}
      </div>
    </div>
  </footer>
</template>

<script>
import config from '../../config.json5';

export default {
  name: 'IPTVFooter',
  data() {
    return {
      mailbox: config.mailbox,
      srcCredit: config.srcCredit,
      wechat: config.wechat,
      telegram: config.telegram,
      sponsor: config.sponsor,
      version: WEBPACK_APP_VERSION,
      built: new Date(WEBPACK_TIMESTAMP),
    };
  },
};
</script>

<style lang="scss">
div#body {
  display: flex;
  min-height: 100vh;
  flex-direction: column;

  main {
    flex: 1 0 auto;
  }
}

</style>
