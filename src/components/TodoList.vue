<template>
	<div class="test">
		<!-- Main Input -->
		<input 
			type="text" 
			class="todo-input" 
			placeholder="What needs to be done" 
			v-model="newTodo" 
			@keyup.enter="addTodo"
		>
		<!-- End Main Input -->
		<!-- For loop -->
		<div 
			v-for="(todo, index) in todos" 
			:key="todo.id" 
			class="todo-item"
		>
			<div class="todo-item-left">
				<!-- CheckBox -->
				<input type="checkbox" v-model="todo.completed">
				<!-- End Check Box -->
				<!-- if state is true -->
				<div 
					v-if="!todo.editing" 
					@dblclick="editTodo(todo)" 
					class="todo-item-label"
					:class="{ 
						completed : todo.completed
					}"
				>
						{{ todo.title }}
				</div>
				<!-- if state is false -->
				<!-- Items Inputs -->
				<input 
					v-else 
					class="todo-item-edit"
					type="text" 
					v-model="todo.title" 
					@blur="doneEdit(todo)" 
					@keyup.enter="doneEdit(todo)"
					@keyup.esc="canselEdit(todo)"
					v-focus
				>

			</div>
			<div class="remove-item" @click="removeTodo(index)">
				&times;
			</div>
		</div>
		<!-- End loop for -->
	</div>	 
</template>

<script>

export default {
	name: 'TodoList',
	data () {
	    return {
		      	newTodo: '',
		      	idForTodo: 3,
		      	beforeEditCache: '',
		      	todos:[
			      	{
			      		'id': 1,
			      		'title': 'Finish Vue Screencast',
			      		'completed': false,
			      		'editing': false
			      	},
			      	{
			      		'id': 2,
			      		'title': 'Take over world',
			      		'completed': false,
			      		'editing': false
			      	}
		    	]
	   }
 	},
 	directives: {
	    focus: {
	            inserted: function (el) {
	            el.focus()
	        }
	    }
	},
 	methods: {
 		addTodo(){

 			if (this.newTodo.trim().length == 0) {
 				return
 			}

 			this.todos.push({
 				id: this.idForTodo,
 				title: this.newTodo,
 				completed: false
 			})

 			this.newTodo = ''
 			this.idForTodo++
 		},
 		editTodo(todo){
 			this.beforeEditCache = todo.title
 			// alert('double clicked');
 			todo.editing = true;
 		},
 		doneEdit(todo){
 			if (todo.title.trim() == '') {
 				todo.title = this.beforeEditCache;
 			}

 			todo.editing = false;
 		},
 		canselEdit(todo){
 			todo.title = this.beforeEditCache
 			todo.editing = false
 		},
 		removeTodo(index){
 			this.todos.splice(index,1);
 		}
 	}
}

</script>

<style lang="scss">

	.todo-input{
		width: 100%;
		padding: 10px 18px;
		font-size: 18px;
		margin-bottom: 16px;

		&:focus{
			outline: 0;
		}
	}

	.todo-item{
		margin-bottom: 12px;
		display: flex;
		align-items:center;
		justify-content: space-between;
	}

	.remove-item{
		cursor: pointer;
		margin-left: 14px;
		&:hover{
			color: black;
		}
	}

	.todo-item-left{
		display: flex;
		align-items: ctnter;
	}

	.todo-item-label{
		padding: 10px;
		border: 1px solid white;
		margin-left: 12px;
	}

	.todo-item-edit{
		font-size: 12px;
		color: #2c3e50;
		margin-left: 12px;
		width: 100%;
		padding: 10px;
		border:  1px solid #ccc;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;

		&:focus{
			outline: none;
		}
	}

	.completed{
		text-decoration: line-through;
		color: grey;
	}
</style>