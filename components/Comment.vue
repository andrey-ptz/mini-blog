<template>
	<div class="comment mb">
    
		<header class="comment__header">
      <h4>{{ comment.name }}</h4>
			<el-button
				title="Удалить комментарий"
				icon="el-icon-delete"
				size="mini"
				type="danger"
				@click="$emit('deleteComment', comment.key)"></el-button>
    </header>

		<div class="comment__description">
			{{ comment.text }}
		</div>
		
  </div>
</template>

<script>
export default {
	props: {
		comment: {
			type: Object,
			required: true
		}
	}
}
</script>

<style lang="sass" scoped>
.comment
	max-width: 600px
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
		padding: 10px 15px
		font-size: .9rem
		border-bottom: 1px solid #ebeef5
	&__description
		padding: 10px 15px
		font-size: .8rem
</style>