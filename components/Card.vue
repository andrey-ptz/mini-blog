<template>
	<div class="card">
    
		<header class="card__header">
      <h4>{{ note.title }}</h4>
			<small title="Количество комментариев">
				<i class="el-icon-chat-square"></i>
				{{ note.comments.length }}
			</small>
    </header>

		<div class="card__description">
			{{ note.description }}
		</div>
		
		<footer class="card__footer">
			<el-button 
				title="Открыть запись"
				icon="el-icon-view" 
				size="mini"
				type="success"
				plain
				@click="$router.push(`/${note.key}`)"
				>Открыть</el-button>
			<div>
				<el-button
					title="Редактировать запись"
					icon="el-icon-edit"
					size="mini"
					type="primary"
					plain
					@click="$router.push(`/edit/${note.key}`)"></el-button>
				<el-button
					title="Удалить запись"
					icon="el-icon-delete"
					size="mini"
					type="danger"
					@click="$emit('deleteNote', note.key)"></el-button>
			</div>
		</footer>
		
  </div>
</template>

<script>
export default {
	props: {
		note: {
			type: Object,
			required: true
		}
	}
}
</script>

<style lang="sass" scoped>
.card
	display: flex
	flex-direction: column
	color: #303133
	border: 1px solid #EBEEF5
	border-radius: 4px
	overflow: hidden
	transition: .3s
	&:hover
		box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1)
	&__header
		display: flex
		justify-content: space-between
		align-items: center
		flex-shrink: 0
		padding: 20px
		border-bottom: 1px solid #ebeef5
	&__description
		font-size: .9rem
		padding: 20px
		border-bottom: 1px solid #ebeef5
		flex-grow: 1
	&__footer
		display: flex
		justify-content: space-between
		align-items: center
		padding: 20px
</style>