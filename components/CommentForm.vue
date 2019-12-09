<template>
	<el-form 
		:model="controls" 
		:rules="rules" 
		ref="form" 
		@submit.native.prevent="action"
	>

		<el-form-item label="Имя комментатора" prop="name">
			<el-input v-model="controls.name" />
		</el-form-item>

		<el-form-item label="Текст комментария" prop="text" class="mb2">
			<el-input 
				v-model="controls.text" 
				type="textarea" 
				resize="none"
				:rows="3"
			/>
		</el-form-item>

		<el-form-item>
			<el-button type="primary" native-type="submit">
				Добавить комментарий
			</el-button>
		</el-form-item>

	</el-form>	
</template>

<script>
export default {
	data() {
		return {
			controls: {
				name: '',
				text: ''
			},
			rules: {
				name: [
					{ required: true, message: 'Имя комментатора не должно быть пустым', trigger: 'blur' },
					{ max: 20, message: 'Максимальная длина 20 символов' }
				],
				text: [
					{ required: true, message: 'Текст комментария не должен быть пустым', trigger: 'blur' },
					{ max: 200, message: 'Максимальная длина 200 символов' }
				]
			}
		}
	},
	methods: {
		action() {
			this.$refs.form.validate(valid => {
				if (valid) {
					const formData = {
						name: this.controls.name.trim(),
						text: this.controls.text.trim()
					}

					this.$emit('createComment', formData)
					this.controls.name = this.controls.text = ''
				}
			})
		}
	}
}
</script>

<style lang="sass" scoped>
form
	max-width: 600px
</style>