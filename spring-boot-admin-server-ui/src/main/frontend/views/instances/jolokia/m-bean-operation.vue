<!--
  - Copyright 2014-2018 the original author or authors.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->

<template>
  <div class="field">
    <div class="control">
      <button class="button is-light is-fullwidth columns has-text-left" @click="execute($event)">
        <small class="is-light is-muted column is-flex-grow-0 is-flex-shrink-0 p-1" v-text="shortenedRet"
               :title="descriptor.ret"/>
        <span class="column is-flex-grow-1 is-flex-shrink-0 p-1 is-truncated" v-text="shortenedName" :title="name"/>
      </button>
      <p class="help" v-text="descriptor.desc"/>
    </div>
    <sba-modal v-model="isModalOpen" data-testid="mBeanOperationModal">
      <template v-slot:header>
        <span v-html="$t('instances.jolokia.execute_modal_header', {name: shortenedName})"/>
      </template>
      <template v-slot:footer>
        <button class="button is-light" @click="closeModal">
          Cancel
        </button>
        <button class="button is-success" @click="executeOperation($event)">
          OK
        </button>
      </template>
    </sba-modal>
  </div>

</template>

<script>
import {truncateJavaType} from '@/views/instances/jolokia/utils';
import SbaModal from '@/components/sba-modal';

export default {
  components: {SbaModal},
  props: {
    name: {
      type: String,
      required: true
    },
    descriptor: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isModalOpen: false,
    }
  },
  computed: {
    shortenedName() {
      return truncateJavaType(this.name);
    },
    shortenedRet() {
      return truncateJavaType(this.descriptor.ret);
    }
  },
  methods: {
    closeModal() {
      this.isModalOpen = false;
    },
    executeOperation(event) {
      this.$emit('click', event);
      this.isModalOpen = false;
    },
    execute(event) {
      if (this.descriptor.args.length === 0) {
        this.isModalOpen = true;
      } else {
        this.executeOperation(event);
      }
    }
  }
}
</script>

<style>
.is-truncated {
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
