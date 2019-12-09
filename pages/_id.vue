<template>
	<div>
		<section class="content mb2">
			<el-breadcrumb separator="/" class="mb2">
				<el-breadcrumb-item to="/">Главная</el-breadcrumb-item>
				<el-breadcrumb-item>{{ note.title }}</el-breadcrumb-item>
			</el-breadcrumb>

			<h1 class="title mb2">{{ note.title }}</h1>

			<div class="text" v-html="note.text"></div>
		</section>

		<section class="comments mb2">
			<h2 class="mb2">Комментарии:</h2>

			<p class="mb2" v-if="!comments.length">Комментариев пока нет</p>

			<div v-else class="comments__container mb2">
				<Comment
					v-for="comment in comments"
					:key="comment.key"
					:comment="comment"
					@deleteComment="deleteComment"
				/>
			</div>

			<h2 class="mb">Добавить комментарий</h2>
			<CommentForm @createComment="createComment" />
		</section>
	</div>
</template>

<script>
import CommentForm from '@/components/CommentForm'
import Comment from '@/components/Comment'

export default {
	components: {
		CommentForm,
		Comment
	},
	head() {
		return {
      title: this.note.title
    }
	},
	validate({ params }) {
		return !!localStorage.getItem(params.id)
	},
	data() {
		return {
			note: null
		}
	},
	created() {
		this.note = JSON.parse(localStorage.getItem(this.$route.params.id))
	},
	computed: {
		comments() {
			const commentsArray = []
			if (this.note.comments.length) {
				this.note.comments.forEach(key => {
					commentsArray.push({
						...JSON.parse(localStorage.getItem(key)),
						key
					})
				})
			}
			return commentsArray
		}
	},
	methods: {
		createComment(formData) {
			const randomKey = `c${(~~(Math.random()*1e8)).toString(16)}`
			this.note.comments.push(randomKey)
			
			try {
				localStorage.setItem(this.$route.params.id, JSON.stringify(this.note))
				localStorage.setItem(randomKey, JSON.stringify(formData))
				this.$message.success('Комментарий успешно добавлен')
			} catch (e) {
				if (e === 'QUOTA_EXCEEDED_ERR') {
					this.$message.error('Превышен лимит хранилища')
				}
			}
		},
		async deleteComment(key) {
			try {
				await this.$confirm('Удалить комментарий?', 'Внимание', {
					confirmButtonText: 'Да',
					cancelButtonText: 'Отменить',
					type: 'warning',
					customClass: 'adaptive'
				})
				// create new array without deleted comment key
				this.note.comments = this.note.comments.filter(k => k !== key)
				// set new value in localStorage
				localStorage.setItem(this.$route.params.id, JSON.stringify(this.note))
				// remove comment from localStorage
				localStorage.removeItem(key)

				this.$message.success('Комментарий успешно удален')
			} catch (e) {}
		}
	}
}
</script>

<style lang="sass">
.title
	font-size: 1.75rem
h2
	font-size: 1.25rem
p
	margin: 20px 0
</style>
