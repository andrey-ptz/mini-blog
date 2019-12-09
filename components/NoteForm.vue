<template>
	<el-form 
		:model="controls" 
		:rules="rules" 
		ref="form" 
		@submit.native.prevent="action"
	>

		<el-breadcrumb separator="/" class="mb2">
			<el-breadcrumb-item to="/">Главная</el-breadcrumb-item>
			<el-breadcrumb-item v-if="type === 'create'">
				Добавление записи
			</el-breadcrumb-item>
			<el-breadcrumb-item v-if="type === 'edit'">
				Редактирование записи
			</el-breadcrumb-item>
		</el-breadcrumb>

		<h1 class="mb" v-if="type === 'create'">Создать новую запись</h1>
		<h1 class="mb" v-if="type === 'edit'">Редактирование записи</h1>

		<el-form-item label="Заголовок" prop="title">
			<el-input v-model="controls.title" />
		</el-form-item>

		<el-form-item label="Краткое описание" prop="description">
			<el-input 
				v-model="controls.description" 
				type="textarea" 
				resize="none"
				:rows="3"
			/>
		</el-form-item>

		<el-form-item label="Полное описание" prop="text" class="mb2">
			<el-input 
				v-model="controls.text" 
				type="textarea" 
				resize="none"
				:rows="10"
			/>
		</el-form-item>

		<el-form-item>
			<el-button type="primary" native-type="submit" v-if="type === 'create'">
				Создать запись
			</el-button>
			<el-button type="primary" native-type="submit" v-if="type === 'edit'">
				Сохранить изменения
			</el-button>
		</el-form-item>

	</el-form>	
</template>

<script>
export default {
	props: {
		type: {
			type: String,
			required: true
		},
		note: {
			type: Object,
			default: () => ({
				title: '',
				description: '',
				text: '',
				comments: []
			})
		}
	},
	data() {
		return {
			controls: {
				title: this.note.title,
				description: this.note.description,
				text: this.note.text
			},
			rules: {
				title: [
					{ required: true, message: 'Заголовок не должен быть пустым', trigger: 'blur' },
					{ max: 20, message: 'Максимальная длина заголовка 20 символов' }
				],
				description: [
					{ required: true, message: 'Краткое описание не должно быть пустым', trigger: 'blur' },
					{ max: 200 }
				],
				text: [
					{ required: true, message: 'Полное описание не должно быть пустым', trigger: 'blur' }
				]
			}
		}
	},
	methods: {
		action() {
			this.$refs.form.validate(valid => {
				if (valid) {
					const formData = {
						title: this.controls.title.trim(),
						description: this.controls.description.trim(),
						text: this.controls.text.trim(),
						comments: this.note.comments
					}

					switch(this.type) {
						case 'create':
							this.$emit('createNote', formData)
							this.controls.title = this.controls.description = this.controls.text = ''
							break
						case 'edit':
							this.$emit('editNote', formData)
							break
					}
				}
			})
		}
	}
}
</script>

<style lang="sass" scoped>
form
	margin: 0 auto
	max-width: 600px
</style>