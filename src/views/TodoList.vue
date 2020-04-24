<template>
  <a-form :form="form" @submit="handleSubmit">
    <FormItem v-for="item in list" :key="item.id" :data="item" @click="remove" />
    <a-form-item>
      <a-button type="dashed" style="width: 60%" @click="add">
        <a-icon type="plus" /> Add field
      </a-button>
    </a-form-item>
    <a-form-item>
      <a-button type="primary" html-type="submit">
        Submit
      </a-button>
    </a-form-item>
  </a-form>
</template>

<script>
import FormItem from '@/components/FormItem.vue'

export default {
  components: {
    FormItem
  },
  data() {
    return {
      list: [
        { id: Date.now(), input: '', textarea: '', showIcon: false },
      ]
    };
  },
  created() {
    this.form = this.$form.createForm(this, {
      name: 'global_state',
      onFieldsChange: (_, changedFields) => {
        this.$emit('change', changedFields);
      },
      mapPropsToFields: () => {
        return {
          list: this.$form.createFormField(this.list),
        };
      },
      onValuesChange: (_, values) => {
        this.list.forEach(item => {
          const [key, id] = Object.keys(values)[0].split('_');
          if (item.id == id) {
            item[key] = values[`${key}_${id}`];
          }
        })
      },
    });
  },
  methods: {
    remove({ id }) {
      this.list = this.list.filter(item => item.id !== id)
    },

    add() {
      this.list.length && (this.list[0].showIcon = true);
      this.list.push({
        id: Date.now(),
        input: '',
        textarea: '',
        showIcon: this.list.length > 0
      })
    },

    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log(values);
          // 其实可以提交list的参数
          console.log(this.list);
        }
      });
    },
  },
};
</script>
<style>
.dynamic-delete-button {
  cursor: pointer;
  font-size: 24px;
  color: #999;
  transition: all 0.3s;
}
.dynamic-delete-button:hover {
  color: #777;
}
.dynamic-delete-button[disabled] {
  cursor: not-allowed;
  opacity: 0.5;
}
</style>