<template>
  <vue-form
    :payload="request"
    @success="postSuccess"
    @notvalid="printErrors"
  >
    <form-field
      v-model="fields.email.value"
      :payload="fields.email"
    />
    <form-field
      v-model="fields.password.value"
      :payload="fields.password"
    />
  </vue-form>
</template>
<script>
import FormMixin from '@/mixins/form'
export default {
    mixins: [FormMixin],
    data() {
        return {
            fields: {
                email: {
                    hint: `Pon tu correo electrónico`,
                    value: ``,
                    error: ``,
                    type: `text`,
                },
                password: {
                    hint: `Pon tu contraseña`,
                    value: ``,
                    error: ``,
                    type: `password`,
                },
            },
        }
    },
    computed: {
        request() {
            const data = {
                email: this.fields.email.value,
                password: this.fields.password.value,
            }
            return {
                method: `post`,
                url: `/auth/signin`,
                data: data,
            }
        },
    },
    methods: {
        async postSuccess(data) {
            this.$api.setHeader(`authorization`, data.token)
            this.user = data.userInfo
            this.token = data.token
            this.resetForm()
        },
    },
}
</script>
