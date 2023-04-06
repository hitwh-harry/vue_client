<template>
  
  <div class="app-container">

    <el-dialog title="" :visible.sync="detailVisible" width="35%">
      次数已用尽
    </el-dialog>

    <h3>可下载次数：{{ times }}</h3>

    <el-button type="primary" style="width:10%;margin-bottom:30px;" 
      @click.native.prevent="downloadFile">下载 {{filename}}
    </el-button>
    
  </div>
</template>

<script>
import { getDownloadInfo} from "@/api/download.js";
import store from '@/store'
import axios from 'axios'

export default {
  data() {
    
    return {
      filename:"",
      times:0,
      detailVisible:false
    };
  },
  created: function () {
    getDownloadInfo().then((response) => {
      this.filename=response.data.filename;
      this.times=response.data.times;
    });
  },
  methods: {
    downloadFile() {
      if(this.times!=0){
        axios.get(process.env.VUE_APP_BASE_API+"/download/downloadFile",{
              params: {
                token: store.getters.token,
              },
              responseType: 'blob'
            }).then(res => {
              const blob = new Blob([res.data]);
              const fileName = res.headers["content-disposition"].split(";")[1].split("filename=")[1];
              if ('download' in document.createElement("a")) {
                const link = document.createElement("a");
                link.download = fileName;
                link.style.display = 'none';
                link.href = URL.createObjectURL(blob);
                document.body.appendChild(link);
                link.click();
                URL.revokeObjectURL(link.href);
                document.body.removeChild(link);
              } else {
                navigator.msSaveBlob(blob, fileName);
              }
              this.$router.go(0);
			  })
      }
      else{
        this.detailVisible=true;
      }
      
		}
    
    
  },
};
</script>
