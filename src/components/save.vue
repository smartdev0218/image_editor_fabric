<template>
  <div class="save-box">
    <Dropdown style="margin-left: 10px" @on-click="saveJson">
      <Button type="primary" size="small" style="font-size:11px" @click="saveJson">
        {{saveType}}
      </Button>
    </Dropdown>
  </div>
</template>

<script>
import select from '@/mixins/select';
import axios from "axios";
export default {
  name: 'saveBar',
  inject:["path","param_id"],
  mixins: [select],
  data() {
    return {
      saveType:"Save Image"
    };
  },

  mounted(){
    if(this.path.slice(8) == "create"){
      this.saveType = "Save Image"
    }else{
      this.saveType = "Edit Image"
    }
    
  },  
  methods: {
    saveWith(type) {
      this[type]();
    },
    // saveJson() {
    //   console.log(this.canvas.c.getObjects())
    //   console.log(this.canvas.c.toJSON())

    //   const dataUrl = this.canvas.editor.getJson();
    //   const fileStr = `data:text/json;charset=utf-8,${encodeURIComponent(
    //     JSON.stringify(dataUrl, null, '\t')
    //   )}`;
    //   this.downFile(fileStr, 'json');
    // },
    // saveImg() {
    //   const workspace = this.canvas.c.getObjects().find((item) => item.id === 'workspace');
    //   const { left, top, width, height } = workspace;
    //   const option = {
    //     name: 'New Image',
    //     format: 'png',
    //     quality: 1,
    //     left,
    //     top,
    //     width,
    //     height,
    //   };
    //   this.canvas.c.setViewportTransform([1, 0, 0, 1, 0, 0]);
    //   const dataUrl = this.canvas.c.toDataURL(option);
    //   this.downFile(dataUrl, 'png');
    // },    
    // downFile(fileStr, fileType) {
    //   const anchorEl = document.createElement('a');
    //   anchorEl.href = fileStr;
    //   anchorEl.download = `${uuid()}.${fileType}`;
    //   document.body.appendChild(anchorEl); // required for firefox
    //   anchorEl.click();
    //   anchorEl.remove();
    // },        
    saveJson() {
      if(this.saveType == "Save Image"){
        const dataUrl = this.canvas.editor.getJson();
        const fileStr = `data:text/json;charset=utf-8,${encodeURIComponent(
          JSON.stringify(dataUrl, null, '\t')
        )}`;
        axios.post("http://localhost:3000/feed-image/create",{data:dataUrl}).then((res)=>{
          alert(res.data)
        })
      }else{
        const dataUrl = this.canvas.editor.getJson();
        const fileStr = `data:text/json;charset=utf-8,${encodeURIComponent(
          JSON.stringify(dataUrl, null, '\t')
        )}`;
        axios.post("http://localhost:3000/feed-image/edit/"+this.param_id,{data:dataUrl}).then((res)=>{
          alert(res.data)
        })
        // this.downFile(fileStr, 'json');        
      }

    }
    }
};
</script>

<style scoped lang="less">
.save-box {
  display: inline-block;
}
</style>
