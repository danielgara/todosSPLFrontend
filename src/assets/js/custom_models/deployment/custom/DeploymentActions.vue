<!--
    @author Daniel Correa <dcorreab@eafit.edu.co>
-->
<template>
  <div class="btn-group" role="group">
    <button class="btn btn-secondary dropdown-toggle" type="button"
      data-toggle="dropdown" aria-expanded="false" aria-haspopup="true" id="btnGroupActions1">
      Derivation
    </button>
    <div id="divDropdownActions" class="dropdown-menu" aria-labelledby="btnGroupActions1">
      <a class="dropdown-item dropdown-pointer" v-on:click="testComponentBackend">
        Test Component Management Backend
      </a>
    </div>
  </div>
</template>

<script lang="ts">
// @ts-nocheck
import { Vue, Options } from 'vue-class-component';
import axios from 'axios';

@Options({
  props: ['variaMosGraph'],
})
export default class DeploymentActions extends Vue {
  public variaMosGraph:any; // VariaMosGraph object

  public mounted() {
    this.customConfig = this.variaMosGraph.configApp.getCustomConfigAsJsonObject().component;
  }

  public testComponentBackend() {
    const modal = this.variaMosGraph.getModal();
    axios.get(`${this.customConfig.backendURL}deployment/test`)
      .then((response) => {
        if (response.data == 'Ok') {
          modal.setData('success', 'Success', 'Backend connection is Ok');
          modal.click();
        } else {
          modal.setData('error', 'Error', 'Wrong backend connection');
          modal.click();
        }
      })
      .catch((error) => {
        modal.setData('error', 'Error', `Wrong backend connection. ${error}`);
        modal.click();
      });
  }
}
</script>
