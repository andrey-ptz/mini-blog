<template>
	<div>
		<NoteForm 
			type="edit"
			:note="note"
			@editNote="editNote" 
		/>	
  </div>
</template>

<script>
import NoteForm from '@/components/NoteForm'

export default {
	components: {
		NoteForm
	},
	head: {
		title: 'Редактирование записи'
	},
	validate({ params }) {
		return !!localStorage.getItem(params.id)
	},
	data() {
		return {
			note: {
				...JSON.parse(localStorage.getItem(this.$route.params.id)),
				key: this.$route.params.id
			}
		}
	},
	methods: {
		editNote(formData) {
			try {
				localStorage.setItem(this.$route.params.id, JSON.stringify(formData))
				this.$message.success('Запись успешно изменена')
			} catch (e) {
				if (e === 'QUOTA_EXCEEDED_ERR') {
					this.$message.error('Превышен лимит хранилища')
				}
			}
		}
	}
}
</script>

<style lang="sass" scoped>

</style>