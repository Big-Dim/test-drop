<template>
  <div>
    <h3>Drop Zone</h3>
    <vue-dropzone ref="myVueDropzone" id="dropzone" 
      @vdropzone-file-added="vfileAdded" 
      @vdropzone-success="vsuccess" 
      @vdropzone-error="verror" 
      @vdropzone-removed-file="vremoved" 
      @vdropzone-sending="vsending" 
      @vdropzone-success-multiple="vsuccessMuliple" 
      @vdropzone-sending-multiple="vsendingMuliple" 
      @vdropzone-queue-complete="vqueueComplete" 
      @vdropzone-total-upload-progress="vprogress" 
      @vdropzone-mounted="vmounted" 
      @vdropzone-drop="vddrop" 
      @vdropzone-drag-start="vdstart" 
      @vdropzone-drag-end="vdend" 
      @vdropzone-drag-enter="vdenter" 
      @vdropzone-drag-over="vdover" 
      @vdropzone-drag-leave="vdleave" 
      @vdropzone-duplicate-file="vdduplicate"
      :options="dropzoneOptions"
      :duplicateCheck="true">
    </vue-dropzone>
    
    <h3>Thumbnails</h3>
	<div v-for="(img, key) in dataimg">
		<div class="gallery">{{img.Size}}<img  :alt="img.Name" :src="img.Src" /></div>
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
      success: false,
      error: false,
      removedFile: false,
      sending: false,
      successMultiple: false,
      sendingMultiple: false,
      queueComplete: false,
      uploadProgress: false,
      progress: false,
      myProgress: 0,
      isMounted: false,
      dDrop: false,
      dStarted: false,
      dEnded: false,
      dEntered: false,
      dOver: false,
      dLeave: false,
      dDuplicate:false,
	  dataimg:[]
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
		const elem = {Name:file.name, Size:size, Type:type, Src:src }; //создаем Объект с нужными свойствами 
		this.dataimg.push(elem); //Заносим объект в массив
		//console.log("dataimg - ", arr, this.dataimg);
		
	  }, 200)
	   
    },
    vfilesAdded(file) {
      this.filesAdded = true
	  
	console.log ('Event : vdropzone-files-added')
    },
    vsuccess(file, response) {
      this.success = true
      console.log('', 'Event : vdropzone-success')
    },
    verror(file) {
      this.error = true
      console.log(file.upload.filename, 'Event : vdropzone-error - ' + file.status)
    },
    vremoved(file, xhr, error) {
      this.removedFile = true
      console.log('', 'Event : vdropzone-removedFile')
    },
    vsending(file, xhr, formData) {
      this.sending = true
      console.log('', 'Event : vdropzone-sending')
    },
    vsuccessMuliple(files, response) {
      this.successMultiple = true
      console.log('', 'Event : vdropzone-success-multiple')
    },
    vsendingMuliple(file, xhr, formData) {
      this.sendingMultiple = true
      console.log('', 'Event : vdropzone-sending-multiple')
    },
    vqueueComplete(file, xhr, formData) {
      this.queueComplete = true
      console.log('', 'Event : vdropzone-queue-complete')
    },
    vprogress(totalProgress, totalBytes, totalBytesSent) {
      this.progress = true
      this.myProgress = Math.floor(totalProgress)
      console.log('', 'Event : vdropzone-sending')
    },
	vmounted() {
      this.isMounted = true
	  console.log('1');
    },
    vddrop() {
      this.dDrop = true
    },
    vdstart() {
      this.dStarted = true
    },
    vdend() {
      this.dEnded = true
    },
    vdenter() {
      this.dEntered = true
    },
    vdover() {
      this.dOver = true
    },
    vdleave() {
      this.dLeave = true
    },
    vdduplicate() {
      this.dDuplicate = true
    }
  },
  
  watch: {
    fileAdded() {
      let that = this
      setTimeout(function() {
        that.fileAdded = false;
      }, 2000)
    },
    filesAdded() {
      let that = this
      setTimeout(function() {
        that.filesAdded = false
      }, 2000)
    },
    success() {
      let that = this
      setTimeout(function() {
        that.success = false
      }, 2000)
    },
    error() {
      let that = this
      setTimeout(function() {
        that.error = false
      }, 2000)
    },
    removedFile() {
      let that = this
      setTimeout(function() {
        that.removedFile = false
      }, 2000)
    },
    sending() {
      let that = this
      setTimeout(function() {
        that.sending = false
      }, 2000)
    },
    successMultiple() {
      let that = this
      setTimeout(function() {
        that.successMultiple = false
      }, 2000)
    },
    sendingMultiple() {
      let that = this
      setTimeout(function() {
        that.sendingMultiple = false
      }, 2000)
    },
    queueComplete() {
      let that = this
      setTimeout(function() {
        that.queueComplete = false
      }, 2000)
    },
    progress() {
      let that = this
      setTimeout(function() {
        that.progress = false
      }, 2000)
    },
    isMounted() {
      let that = this
	  
	  this.dataimg = new Array();// начальная инициализация
      setTimeout(function() {
        that.isMounted = false
      }, 2000)
    },
    dDrop() {
      let that = this
      setTimeout(function() {
        that.dDrop = false
      }, 2000)
    },
    dStarted() {
      let that = this
      setTimeout(function() {
        that.dStarted = false
      }, 2000)
    },
    dEnded() {
      let that = this
      setTimeout(function() {
        that.dEnded = false
      }, 2000)
    },
    dEntered() {
      let that = this
      setTimeout(function() {
        that.dEntered = false
      }, 2000)
    },
    dOver() {
      let that = this
      setTimeout(function() {
        that.dOver = false
      }, 2000)
    },
    dLeave() {
      let that = this
      setTimeout(function() {
        that.dLeave = false
      }, 2000)
    },
    dDuplicate() {
      let that = this
      setTimeout(function() {
        that.dDuplicate = false
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





