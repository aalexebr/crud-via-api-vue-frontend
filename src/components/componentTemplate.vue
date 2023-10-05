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
		recieved:null,
		openUpdateForm: false,
		showApiFile : '',
		editFormData:{
			name: '',
			file: null,
			remove:false
		},
		updateFile1: null,
	  }
    },
    components: {

    },
	methods:{
		storeFile(){
			// let formData = new FormData();
			// formData.append('file', this.file);
			axios.post('http://127.0.0.1:8000/api/file',this.formData,{
    			headers: {
        			'Content-Type': 'multipart/form-data'
    					}
  				}).then((res)=>{
					// console.log(this.file)
					console.log('create success',res.data);
				})
				.catch((err)=>{
					console.log(err)
				})
		},
		getFiles(){
			axios.get('http://127.0.0.1:8000/api/file')
				.then((res)=>{
					console.log('get file success',res.data)
						this.recieved = res.data.results;
					})
					.catch((err)=>{
					console.log(err)
				})
		},
		uploadFile(event){
			this.file = event.target.files[0];
			this.formData.file = event.target.files[0];
			// console.log(event.target)
			
		},
		getUpdateForm(id){
			this.openUpdateForm = true;
			this.showApiFile = ''
			axios.get(`http://127.0.0.1:8000/api/file/${id}`)
				.then((res)=>{
					console.log('update req success',res.data.result);
					this.showApiFile = res.data.result;
				})
				.catch((err)=>{
					console.log(err)
				})
		},
		editFile(event){
			this.editFile1 = event.target.files[0];
			this.editFormData.file = event.target.files[0];
			// console.log(event.target)
			
		},
		updateFile(){
			// axios.put with a file doesn't work so 
			console.log(this.editFormData)
			this.editFormData.name = this.showApiFile.name
			axios.post(`http://127.0.0.1:8000/api/file/${this.showApiFile.id}`,this.editFormData,
				{
    			headers: {
        			'Content-Type': 'multipart/form-data'
    					}
  				}
				)
				.then((res)=>{
					console.log('new fake pur success',res.data);
				})
				.catch((err)=>{
					console.log(err)
				})
		},
		deleteFile(id){
			console.log('file deleted')
			axios.delete(`http://127.0.0.1:8000/api/file/${id}`)
				.then((res)=>{
						console.log('delete success',res.data);
					})
				.catch((err)=>{
					console.log(err)
				})
		}
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
  <!-- api/index GET -->
  <button @click="getFiles">getfiles</button>
  <template v-if="recieved != null">
	<ul>
		<li v-for="(file, i) in recieved" :key="i">
			{{ file.name }} 
			<button @click="getUpdateForm(file.id)">update</button>
			<button @click="deleteFile(file.id)">delete</button>
		</li>
	</ul>
  </template>
  <!-- api/edit from the api.show-->
  <template v-if="openUpdateForm">
	<form @submit.prevent="updateFile">
		<div>
			<label for="name">type in file name</label>
			<input type="text" id="name" name="name" required v-model="showApiFile.name">
		</div>
		<div>
			<input type="file" name="file" @change="editFile($event)">
		</div>
		<div>
			<input type="checkbox" name="remove_file" id="" v-model="editFormData.remove">remove file
		</div>
		<button type="submit">submit</button>
    </form>
  </template>
</template>

<style lang="scss" scoped>

</style>
