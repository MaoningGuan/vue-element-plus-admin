<script setup lang="tsx">
import { Descriptions } from '@/components/Descriptions'
import { useI18n } from '@/hooks/web/useI18n'
import { ref, reactive } from 'vue'
import { Form, FormSchema } from '@/components/Form'
import { Dialog } from '@/components/Dialog'
import { useValidator } from '@/hooks/web/useValidator'
import { useForm } from '@/hooks/web/useForm'
import { DescriptionsSchema } from '@/components/Descriptions'

const { required } = useValidator()

const { t } = useI18n()

const dialogVisible2 = ref(false)

// 页面mount的时候调用fetchData来获取数据显示
// 模拟API请求
// const fetchData = () => {
//   setTimeout(() => {
//     detailData.value = [
//       { label: '创建时间', value: '2023-10-01 08:00:00' },
//       { label: '状态', value: '已发布', type: 'tag', tagType: 'success' },
//       { label: '文档链接', value: 'https://vue-element-plus-admin.com', type: 'link' },
//       { label: '描述', value: '基于Vue3 + Element Plus的后台管理系统模板' }
//     ]
//     loading.value = false
//   }, 1000)
// }

// onMounted(() => {
//   fetchData()
// })

const data = reactive({
  username: 'chenkl',
  nickName: '梦似花落。',
  age: 26,
  phone: '13655971xxxx',
  email: '502431556@qq.com',
  addr: '这是一个很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长的地址',
  sex: '男',
  certy: '3505831994xxxxxxxx'
})

const schema = reactive<DescriptionsSchema[]>([
  {
    field: 'username',
    label: t('descriptionsDemo.username')
  },
  {
    field: 'nickName',
    label: t('descriptionsDemo.nickName')
  },
  {
    field: 'phone',
    label: t('descriptionsDemo.phone')
  },
  {
    field: 'email',
    label: t('descriptionsDemo.email')
  },
  {
    field: 'addr',
    label: t('descriptionsDemo.addr'),
    span: 24
  }
])

const schema2 = reactive<FormSchema[]>([
  {
    field: 'username',
    label: t('descriptionsDemo.username'),
    component: 'Input',
    value: data.username
  },
  {
    field: 'nickName',
    label: t('descriptionsDemo.nickName'),
    component: 'Input',
    value: data.nickName
  },
  {
    field: 'phone',
    label: t('descriptionsDemo.phone'),
    component: 'Input',
    value: data.phone
  },
  {
    field: 'email',
    label: t('descriptionsDemo.email'),
    component: 'Input',
    value: data.email
  },
  {
    field: 'addr',
    label: t('descriptionsDemo.addr'),
    component: 'Input',
    componentProps: {
      type: 'textarea',
      rows: 2
    },
    value: data.addr
  }
])

const rules = reactive({
  username: [required()],
  nickName: [required()],
  phone: [required()],
  email: [required()],
  addr: [required()]
})

const { formRegister, formMethods } = useForm()
const { getElFormExpose, getFormData } = formMethods

const formSubmit = async () => {
  const elForm = await getElFormExpose()
  const valid = await elForm?.validate().catch((err) => {
    console.log(err)
  })
  if (valid) {
    const formData = await getFormData()
    console.log(formData)
    Object.assign(data, formData)
    dialogVisible2.value = false
  }
}
</script>

<template>
  <ContentWrap :title="t('dialogDemo.dialog')" :message="t('dialogDemo.dialogDes')">
    <Descriptions
      :title="t('descriptionsDemo.descriptions')"
      :message="t('descriptionsDemo.descriptionsDes')"
      :data="data"
      :schema="schema"
    />
    <div class="text-center mt-10px">
      <BaseButton type="primary" @click="dialogVisible2 = !dialogVisible2">
        {{ t('exampleDemo.edit') }}
      </BaseButton>
    </div>

    <Dialog v-model="dialogVisible2" :title="t('dialogDemo.dialog')">
      <Form :schema="schema2" :rules="rules" @register="formRegister" />
      <template #footer>
        <BaseButton type="primary" @click="formSubmit">{{ t('dialogDemo.submit') }}</BaseButton>
        <BaseButton @click="dialogVisible2 = false">{{ t('dialogDemo.close') }}</BaseButton>
      </template>
    </Dialog>
  </ContentWrap>
</template>

<style lang="less" scoped>
:deep(.is-required--item) {
  position: relative;

  &::before {
    margin-right: 4px;
    color: var(--el-color-danger);
    content: '*';
  }
}
</style>
