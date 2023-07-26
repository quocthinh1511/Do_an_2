<template>

    <div class="voice">
      <button type="button" class="btn btn-primary  btn-sm speech-to-txt "  v-bind:class="{ 'btn-danger': is_record }" @click="startSpeechToTxt">
          <span > </span> Record
        </button>       
      <p class="record-span">{{transcription_}}</p>
  </div>

  </template>


  <style> 
div.voice
{
    display: block;
    text-align: center;
}
.speech-to-txt
{
    display: inline-block;
    margin-left: auto;
    margin-right: auto;
    text-align: left;
    padding: 20px;
}
.record-span{
    margin-top: 20px;
}
.record_img{

}

</style>
  
  
   <script>
  
    export default {
     name: 'speech_to_text',
     data() {
       return {
         runtimeTranscription_: "",
         transcription_: "",
         lang_: "es-ES", 
         is_record : false
       };
     },
     methods: {
      startSpeechToTxt() {
      // initialisation of voicereco
      this.is_record = true;
      window.SpeechRecognition =
      window.SpeechRecognition || 
      window.webkitSpeechRecognition;
      const recognition = new window.SpeechRecognition();
      recognition.lang = this.lang_;
      recognition.interimResults = true;
  
      // event current voice reco word
      recognition.addEventListener("result", event => {      
        var text = Array.from(event.results)
          .map(result => result[0])
          .map(result => result.transcript)
          .join("");
        this.runtimeTranscription_ = text;
      });
      // end of transcription
      recognition.addEventListener("end", () => {
        this.transcription_ = this.runtimeTranscription_;
        this.runtimeTranscription_ = "";
        recognition.stop();
        this.is_record = false;
      });
       recognition.start();
     },
  
     }
    }
    </script>