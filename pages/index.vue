<template>
	<div>
		<div class="no-cards" v-if="!notes.length">
			<p>На данный момент нет ни одной записи</p>
		</div>

		<div class="cards" v-else>
			<Card 
				v-for="note in notes"
				:key="note.key"
				:note="note"
				@deleteNote="deleteNote"
			/>
		</div>
	</div>
</template>

<script>
import Card from '@/components/Card'

export default {
	components: {
		Card
	},
	head: {
		title: 'Мини блог'
	},
	data() {
		return {
			noteKeys: null,
			autoLoad: false,
			defaultSize: 18,
			autoLoadSize: 6,
			currentSize: 0,
			notes: []
		}
	},
	mounted() {
		window.addEventListener("scroll", this.loadMore)

		// get array of note keys from localStorage
		const noteKeys = JSON.parse(localStorage.getItem('noteKeys'))
		this.noteKeys = noteKeys
		
		this.calcNotes('firstLoad')
	},
	methods: {
		calcNotes(type) {
			let array
			switch(type) {
				case 'firstLoad':
					if (this.defaultSize >= this.noteKeys.length) {
						array = this.noteKeys
						this.currentSize = this.noteKeys.length
					} else {
						array = this.noteKeys.slice(0, this.defaultSize)
						this.autoLoad = true
						this.currentSize = this.defaultSize
					}
					break
				case 'autoLoad':
					if (this.currentSize + this.autoLoadSize >= this.noteKeys.length) {
						array = this.noteKeys.slice(this.currentSize, this.noteKeys.length)
						this.currentSize = this.noteKeys.length
						this.autoLoad = false
					} else {
						array = this.noteKeys.slice(this.currentSize, this.currentSize + this.autoLoadSize)
						this.currentSize += this.autoLoadSize
						this.autoLoad = true
					}
					break
			}
			this.getNotes(array)
		},
		getNotes(array) {
			// get notes from localStorage
			array.forEach(key => this.notes.push({
					...JSON.parse(localStorage.getItem(key)),
					key
				})
			)
		},
		loadMore() {
			if (this.autoLoad) {
				const container = document.querySelector('.container')
				// if the page down
				if (window.pageYOffset + window.innerHeight >= container.offsetHeight) {
					this.calcNotes('autoLoad')
				}
			}
		},
		async deleteNote(key) {
			try {
				await this.$confirm('Удалить запись?', 'Внимание', {
					confirmButtonText: 'Да',
					cancelButtonText: 'Отменить',
					type: 'warning',
					customClass: 'adaptive'
				})
				// get array of note keys from localStorage
				const noteKeys = JSON.parse(localStorage.getItem('noteKeys'))
				// create new array without deleted note key
				const newNoteKeys = noteKeys.filter(k => k !== key)
				// set new value in localStorage
				localStorage.setItem('noteKeys', JSON.stringify(newNoteKeys))
				// remove comments for this note
				const currentNote = JSON.parse(localStorage.getItem(key))
				if (currentNote.comments.length) {
					currentNote.comments.forEach(commentKey => {
						localStorage.removeItem(commentKey)
					})
				}
				// remove note from localStorage
				localStorage.removeItem(key)
				// filter array of notes in current page
				this.notes = this.notes.filter(note => note.key !== key)

				this.$message.success('Запись успешно удалена')
			} catch (e) {}
		}
	}
}
</script>

<style lang="sass">
.cards
	display: grid
	grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))
	grid-gap: 1rem
</style>