<template>
	<button @click="handlePostCreate">Post</button>
	<ul>
		<li v-for="post in posts" v-text="post.title" :key="post.id"></li>
	</ul>
</template>

<script setup>
import { ref, onMounted } from "vue";

const posts = ref([]);

onMounted(() => {
	fetch("http://127.0.0.1:8000/graphql", {
		method: "POST",
		headers: {
			"Content-Type": "application/json",
		},
		body: JSON.stringify({
			query: `query{
  posts{
    data{
      title
      desc
      id
    }
  }
}`,
		}),
	})
		.then(res => res.json())
		.then(res => {
			console.log(res.data);

			posts.value = res.data.posts.data;
		});
});

const handlePostCreate = () => {
	fetch("http://127.0.0.1:8000/graphql", {
		method: "POST",
		headers: {
			"Content-Type": "application/json",
		},
		body: JSON.stringify({
			query: `mutation{
  createPost(user_id:1,title:"vue",desc:"from vue"){
    title
    desc
  }
}`,
		}),
	})
		.then(res => res.json())
		.then(res => {
			console.log(res.data);

			posts.value = res.data.posts.data;
		});
};
</script>
