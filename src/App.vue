<template>
  <div>
    <h3>Drop Zone</h3>
    <vue-dropzone ref="myVueDropzone" id="dropzone" 
      @vdropzone-file-added="vfileAdded" 
      :options="dropzoneOptions"
      :duplicateCheck="true">
    </vue-dropzone>
	
          
    <h3>Thumbnails (click on image for view exif data)</h3>
	 <div v-for="(img, key) in dataimg">    
		<div class="gallery" >{{img.Size}}<img :alt="img.Name" :src="img.Src" @click="showExif(key)" /></div>		
	</div>
<br/>
	
  </div>
  
</template>

<script>
import vueDropzone from 'vue2-dropzone';
import editDoc from './components/DocEditLink.vue';

export default {
  data() {
    return {
      ok: true,
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 100,
        maxFilesize: 0.5,
        headers: { "My-Awesome-Header": "header value" },
        addRemoveLinks: true,
        autoProcessQueue: false,
        accept(file, done) {
          done();
        },
      },
      fileAdded: false,
      filesAdded: false,
	  dataimg:[],
	  exif:{},
    }
  },
  components: {
    vueDropzone,
    'edit-doc': editDoc
  },
  methods: {
    vfileAdded(file) {
      this.fileAdded = true
	  
	  setTimeout(()=>{
		let src = "./src/assets/fileicon_bg.png";// Если тип неопределен
		let type = file.type.split("/")[1];
		switch (type) { //выбираем иконку для типа файла
			case "gif":
			case "png":
			case "jpeg":
			case "jpg":
				src = document.querySelector("[alt=\""+file.name+"\"]").src; //Если картинка - выводим изображение
				break;
			case "pdf":
				src = "./src/assets/pdf.png";
				break;
			case "plain":
				src = "./src/assets/text.png";
				break;			
			case "html":
				src = "./src/assets/html.png";
				break;			
			case "msword":
				src = "./src/assets/word.png";
				break;			
			case "msexcel":
				src = "./src/assets/excel.png";
				break;			
			//... Ну и так далее
				
		}
		let size = " " + file.size + " Bytes"; //размер файла
		let exifData;
		let dataimg = this.dataimg;
		EXIF.getData( file, function() {
            exifData = EXIF.pretty(this);
            if (exifData) {
                ;
            } else {
                exifData = "No EXIF data " ;
            } 
			const elem = {Name:file.name, Size:size, Type:type, Src:src, exifData:exifData}; //создаем Объект с нужными свойствами 
			dataimg.push(elem); //Заносим объект в массив
			
        }); 

		
		
		
	  }, 200)
	   
    },
    vfilesAdded(file) {
      this.filesAdded = true
    },
	showExif(key){	
		alert(this.dataimg[key].exifData);
	}
  },
  
  watch: {
    isMounted() {
      let that = this
	  this.dataimg = new Array();// начальная инициализация
      setTimeout(function() {
        that.isMounted = false
      }, 2000)
    }
  }
}
</script>

<style scoped>
.active {
  color: #78CB5B;
}

.inactive {
  color: #fff000;
}

.fa.fa-circle:before {
  content: "\25C9";
  /*color: #000;*/
}

th {
  text-align: center;
}

td:nth-child(1) {
  text-align: center;
}

td:nth-child(3) {
  text-align: center;
}

td:nth-child(2) {
  padding-left: 40px;
}

.event-active {
  font-weight: bold;
  color: #78CB5B;
  text-transform: uppercase;
  letter-spacing: 1.2px;
}

.tumb {
   display: inline-block;
}

div.gallery {
    margin: 5px;
    border: 1px solid #ccc;
    float: left;
    width: 120px;
	height: 200px;
	overflow: hidden;
}

div.gallery:hover {
    border: 1px solid #777;
}

div.gallery img {
    width: 90%;
    height: auto;
}

div.desc {
    padding: 15px;
    text-align: center;
}



</style>





