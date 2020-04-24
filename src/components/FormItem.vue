<template>
  <div>
    <a-form-item>
      <a-input
        v-decorator="[`input_${data.id}`]"
        style="width: 60%; margin-right: 8px"
      />
      <a-icon
        v-if="data.showIcon"
        class="dynamic-delete-button"
        type="minus-circle-o"
        :disabled="!data.showIcon"
        @click="$emit('click', data)"
      />
      <a-textarea
        v-decorator="[`textarea_${data.id}`]"
        style="width: 60%; margin-right: 8px"
      />
    </a-form-item>
  </div>
</template>
<script>
export default {
  name: 'FormItem',
  props: ['data'],
  data() {
    return {
      test: {}
    }
  }
}
</script>