<template>
  <div class="container">
    <item v-for="story in stories" v-bind:key="story.data.id" v-bind:story="story"></item>
  </div>
</template>

<script>
import axios from 'axios'
import Item from '@/views/Item.vue'

export default {
	name: 'home',
	components: {
		Item
	},
	data() {
		return {
			err: '',
			stories: []
		}
	},
	created() {
		axios
			.get('https://hacker-news.firebaseio.com/v0/topstories.json')
			.then(result => {
				this.results = result.data.slice(0, 10)
				this.results.forEach(element => {
					axios
						.get('https://hacker-news.firebaseio.com/v0/item/' + element + '.json')
						.then(result => {
							this.stories.push(result)
						})
						.catch(err => {
							console.log(err)
						})
				})
			})
			.catch(err => {
				this.err = err
			})
	}
}
</script>