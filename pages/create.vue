<template>
	<div>
		<NoteForm 
			type="create"
			@createNote="createNote" 
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
		title: 'Добавление записи'
	},
	methods: {
		createNote(formData) {
			// random key for example: f5d47a64
			const randomKey = `n${(~~(Math.random()*1e8)).toString(16)}`

			const noteKeys = localStorage.getItem('noteKeys')
				? JSON.parse(localStorage.getItem("noteKeys"))
				: []
			noteKeys.push(randomKey)

			try {
				localStorage.setItem('noteKeys', JSON.stringify(noteKeys))
				localStorage.setItem(randomKey, JSON.stringify(formData))
				this.$message.success('Запись успешно добавлена')
			} catch (e) {
				if (e === 'QUOTA_EXCEEDED_ERR') {
					this.$message.error('Превышен лимит хранилища')
				}
			}
		}
	}
}
</script>