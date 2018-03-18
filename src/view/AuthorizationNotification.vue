<template>
  <div
    id="auth-notification"
    class="modal">
    <div class="modal-content flow-text">
      <h4 v-if="auth">Acesso não autorizado</h4>
      <h4 v-else>Falha na rede</h4>
      <p v-if="auth">
        Alguns recursos são inacessíveis. Pode ser porque você está atualmente fora do campus ou configurou um agente incorretamente.
         Estamos a ponto de iniciar sessão na plataforma de autenticação unificada da sua escola.
         Depois de efetuar login com sucesso, você pode retornar à página atual para continuar assistindo.
         Se o erro acima ainda ocorrer depois que o login for bem-sucedido, você pode tentar configurar o navegador para abrir o cookie de três vias.
      </p>
      <p v-else>Alguns recursos não podem ser acessados. Tente novamente mais tarde.</p>
    </div>
    <div class="modal-footer">
      <a
        v-if="auth"
        href="#!"
        class="modal-action modal-close waves-effect waves-green btn-flat"
        @click="doLogin">Saltar</a>
      <a
        href="#!"
        class="modal-action modal-close waves-effect waves-green btn-flat">
        Cancelamento
      </a>
      <a
        href="#!"
        class="modal-action modal-close waves-effect waves-green btn-flat"
        @click="dismissed=true">
        Nunca mostre novamente
      </a>
    </div>
  </div>
</template>

<script>
import jQuery from 'jquery';

import config from '../../config.json5';

export default {
  name: 'AuthorizationNotification',
  data() {
    return {
      dismissed: false,
      auth: !!config.authUrl,
    };
  },
  mounted() {
    jQuery('#auth-notification').modal({
      dismissible: false,
      opacity: .5,
    });
  },
  methods: {
    notify() {
      if (!this.dismissed) {
        jQuery('#auth-notification').modal('open');
      }
    },
    doLogin() {
      window.location.assign(config.authUrl);
    },
  },
};
</script>
