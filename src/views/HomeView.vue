<template>
  <v-card loading title="Total tab switching" variant="outlined">
    <div class="d-flex justify-center">
      <span class="text-red"> Tab Switch Count: {{ TabSwitchCount }} </span>
      <video ref="video" hidden></video>
    </div>
  </v-card>

  <v-container fluid class="mt-20">
    <v-row>
      <v-col cols="12" lg="4">
        <v-card
          variant="outlined"
          title="Section A"
          text="
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
        
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
       
          ">
        </v-card>
      </v-col>
      <v-col cols="12" lg="4">
        <v-card
          variant="outlined"
          title="Section B"
          text="
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
        
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Earum sed
          placeat natus! Error, totam! Cupiditate atque provident quos! Optio
          qui ipsam officiis pariatur nisi ducimus nobis quisquam ratione neque
          odio.
       
          ">
        </v-card>
      </v-col>
      <v-col cols="12" lg="4">
        <v-card variant="outlined" title="Card title">
          <v-textarea
            bg-color="black"
            class="mr-5 ml-5"
            clearable
            v-model="code.codewrite"
            label="Write Code Here"></v-textarea>
          <v-card-actions>
            <v-btn
              prepend-icon="mdi-code-tags"
              variant="tonal"
              @click="codesubmit"
              >Submit</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      mediaRecorder: null,
      recordedChunks: [],
      stream: null,
      TabSwitchCount: 0,
      code: {
        codewrite: "",
        TabSwitchCount: "",
      },
    };
  },
  async mounted() {
    document.addEventListener("visibilitychange", this.visibilityChangeTab);
    try {
      this.stream = await navigator.mediaDevices.getUserMedia({
        video: true,
      });
      this.$refs.video.srcObject = this.stream;

      this.mediaRecorder = new MediaRecorder(this.stream);

      this.mediaRecorder.addEventListener("dataavailable", (event) => {
        if (event.data.size > 0) {
          this.recordedChunks.push(event.data);
        }
      });

      this.mediaRecorder.addEventListener("stop", () => {
        this.downloadRecording();
      });

      this.mediaRecorder.start();
    } catch (error) {
      console.error("Failed to access webcam:", error);
    }
  },
  methods: {
    visibilityChangeTab() {
      if (document.visibilityState === "hidden") {
        this.TabSwitchCount++;
      }
    },
    async codesubmit() {
      const code = await axios.post("http://localhost:3000/code", {
        codewrite: this.code.codewrite,
        TabSwitchCount: this.TabSwitchCount,
      });
      this.stopvideo();
    },
    downloadRecording() {
      const blob = new Blob(this.recordedChunks, { type: "video/webm" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "recording.webm";
      a.click();
      URL.revokeObjectURL(url);
    },
    stopvideo() {
      if (this.stream) {
        this.stream.getTracks().forEach((track) => track.stop());
      }
      if (this.mediaRecorder && this.mediaRecorder.state !== "inactive") {
        this.mediaRecorder.stop();
      }
    },
  },
  beforeUnmount() {
    if (this.stream) {
      this.stream.getTracks().forEach((track) => track.stop());
    }
    this.stopvideo();
  },
};
</script>
