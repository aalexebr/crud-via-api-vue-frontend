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
			remove:null
		},
		updateFile1: null
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
					console.log(this.file)
					console.log(res.data);
				})
		},
		getFiles(){
			axios.get('http://127.0.0.1:8000/api/file')
				.then((res)=>{
					console.log(res.data)
						this.recieved = res.data.results;
					})
		},
		uploadFile(event){
			this.file = event.target.files[0];
			this.formData.file = event.target.files[0];
			console.log(event.target)
			
		},
		getUpdateForm(id){
			this.openUpdateForm = true;
			this.showApiFile = ''
			axios.get(`http://127.0.0.1:8000/api/file/${id}`)
				.then((res)=>{
					console.log(res.data.result);
					this.showApiFile = res.data.result;
				})
		},
		editFile(event){
			this.editFile1 = event.target.files[0];
			this.editFormData.file = event.target.files[0];
			console.log(event.target)
			
		},
		updateFile(){
			console.log(this.editFormData.remove)
			axios.post(`http://127.0.0.1:8000/api/file/${id}`,this.editFormData,{
    			headers: {
        			'Content-Type': 'multipart/form-data'
    					}
  				}).then((res)=>{
					console.log(this.file)
					console.log(res.data);
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
			{{ file.name }} <button @click="getUpdateForm(file.id)">update</button>
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
