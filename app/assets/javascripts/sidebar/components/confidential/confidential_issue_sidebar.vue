<script>
  import Flash from '../../../flash';
  import editForm from './edit_form.vue';
  import Icon from '../../../vue_shared/components/icon.vue';

  export default {
    components: {
      editForm,
      Icon,
    },
    props: {
      isConfidential: {
        required: true,
        type: Boolean,
      },
      isEditable: {
        required: true,
        type: Boolean,
      },
      service: {
        required: true,
        type: Object,
      },
    },
    data() {
      return {
        edit: false,
      };
    },
    computed: {
      confidentialityIcon() {
        return this.isConfidential ? 'eye-slash' : 'eye';
      },
    },
    methods: {
      toggleForm() {
        this.edit = !this.edit;
      },
      updateConfidentialAttribute(confidential) {
        this.service.update('issue', { confidential })
          .then(() => location.reload())
          .catch(() => {
            Flash(`Something went wrong trying to
  change the confidentiality of this issue`);
          });
      },
    },
  };
</script>

<template>
  <div class="block issuable-sidebar-item confidentiality">
    <div class="sidebar-collapsed-icon">
      <icon
        :name="confidentialityIcon"
        :size="16"
        aria-hidden="true"
      />
    </div>
    <div class="title hide-collapsed">
      Confidentiality
      <a
        v-if="isEditable"
        class="pull-right confidential-edit"
        href="#"
        @click.prevent="toggleForm"
      >
        {{ __('Edit') }}
      </a>
    </div>
    <div class="value sidebar-item-value hide-collapsed">
      <editForm
        v-if="edit"
        :toggle-form="toggleForm"
        :is-confidential="isConfidential"
        :update-confidential-attribute="updateConfidentialAttribute"
      />
      <div
        v-if="!isConfidential"
        class="no-value sidebar-item-value">
        <icon
          name="eye"
          :size="16"
          aria-hidden="true"
          class="sidebar-item-icon inline"
        />
        Not confidential
      </div>
      <div
        v-else
        class="value sidebar-item-value hide-collapsed">
        <icon
          name="eye-slash"
          :size="16"
          aria-hidden="true"
          class="sidebar-item-icon inline is-active"
        />
        This issue is confidential
      </div>
    </div>
  </div>
</template>
