<script>
import axios from 'axios';
export default{
    data(){
      return{
		formData:{
			name: '',
			file: ''
		},
		file:'',
		recieved:''
	  }
    },
    components: {

    },
	methods:{
		storeFile(){
			let formData = new FormData();
			formData.append('file', this.file);
			axios.post('http://127.0.0.1:8000/api/file',this.formData,{
    			headers: {
        			'Content-Type': 'multipart/form-data'
    					}
  				}).then((res)=>{
					console.log(this.file)
					console.log(res.data);
				})
		},
		// sendx(){
		// 	axios.get('http://127.0.0.1:8000/api/file',this.formData)
		// 		.then((res)=>{
		// 			console.log(res)
		// 				// this.recieved = res.data.results;
		// 			})
		// },
		// send(){
		// 	axios.post('http://127.0.0.1:8000/api/file',{name:'hello'})
		// 	.then((res)=>{
		// 			console.log(res);
		// 		})
		// 	.catch(error=>{
		// 		console.log(error)
		// 	})
		// },
		uploadFile(event){
			this.file = event.target.files[0];
			this.formData.file = event.target.files[0];
			// console.log(this.file)
			// console.log(this.formData)
			
		},
	}
}
</script>

<template>
  <div>
    <form @submit.prevent="storeFile">
		<div>
			<label for="name">type in file name</label>
			<input type="text" id="name" name="name" required v-model="formData.name">
		</div>
		<div>
			<input type="file" name="file" @change="uploadFile($event)">
		</div>
		<button type="submit">submit</button>
    </form>
  </div>
  <!-- <button @click="send">click</button> -->
</template>

<style lang="scss" scoped>

</style>
