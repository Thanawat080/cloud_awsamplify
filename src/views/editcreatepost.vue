<template>
  <div class="container">
    <div class="col-12">
      <div class="form-floating mb-3">
        <input
          type="text"
          class="form-control"
          id="floatingInput"
          placeholder="Title"
          v-model="title"
          maxlength="80"
        />
        <label for="floatingInput">Title</label>
      </div>
      <div class="form-floating">
        <textarea
          class="form-control"
          placeholder="Leave a comment here"
          id="floatingTextarea"
          style="height: 300px"
          v-model="description"
        ></textarea>
        <label for="floatingTextarea">What's on your mind?</label>
      </div>
      <br />
      <div class="row">
        <div class="col-3">
          <button type="button" class="btn btn-light">เพิ่มรูป +</button>
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col-3">
          <img
            src="https://bahmansport.com/media/com_store/images/empty.png"
            class="rounded float-start"
            width="200px"
            height="200px"
          />
        </div>
        <div class="col-3">
          <img
            src="https://bahmansport.com/media/com_store/images/empty.png"
            class="rounded float-start"
            width="200px"
            height="200px"
          />
        </div>
        <div class="col-3">
          <img
            src="https://bahmansport.com/media/com_store/images/empty.png"
            class="rounded float-start"
            width="200px"
            height="200px"
          />
        </div>
        <div class="col-3">
          <img
            src="https://bahmansport.com/media/com_store/images/empty.png"
            class="rounded float-start"
            width="200px"
            height="200px"
          />
        </div>
      </div>
      <br />
      <div class="form-floating">
        <select class="form-select" @change="onChange($event)">
          <option value="ลดความอ้วน">ลดความอ้วน</option>
          <option value="หุ่นดี">หุ่นดี</option>
          <option value="Sixpack">Sixpack</option>
          <option value="ไม่ใช้อุปกรณ์">ไม่ใช้อุปกรณ์</option></select
        ><label for="floatingSelect">Tags</label>
      </div>
      
      <br />
      <span v-if="error != false">{{ error }}</span>
      <div class="row">
        <div class="col-11">
          <div class="form-floating mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Custom tag"
              aria-label="Custom tag"
              aria-describedby="basic-addon1"
              v-model="customtag"
              maxlength="20"
            />
            <label for="floatingInput">Custom Tags</label>
          </div>
        </div>
        <div class="col-1">
          <button
            class="btn btn-outline-secondary"
            type="button"
            id="button-addon2"
            @click="confirmcustom"
          >
            เพิ่ม
          </button>
        </div>
      </div>

      <div class="row">
        <div class="col-2"  v-for="(value, index) in tag"
        :key="index"
        @click="cancletag(index)">
        <button type="button" class="btn btn-outline-danger">{{ value }}</button>
        </div>
      </div>

      
      <br /><br />
      <div class="row">
        <div class="col-6">
          <button type="button" class="btn btn-info" @click="editpost()">
            Edit Post
          </button>
        </div>
        <div class="col-6">
          <button type="button" class="btn btn-danger">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      title: "",
      description: "",
      customtag: "",
      tag: [],
      oldpost: []
    };
  },
  props: {
    minLength: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    error() {
      if (this.customtag.length > 20) {
        return `customtag must be less than 20 characters.`;
      }
      return false;
    },
  },

  methods: {
    editpost() {
      axios
        .put("https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/post/" + this.$route.params.id, {
          userid: this.oldpost.userid,
          title: this.title,
          description: this.description,
          tag: this.tag,
          like: this.oldpost.like,
          comment: this.oldpost.comment,
          report: this.oldpost.report
        },{withCredentials: false})
        .then((res) => {
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    onChange(event) {
      if (this.tag.indexOf(event.target.value) == -1) {
        this.tag.push(event.target.value);
      }
    },
    cancletag(index) {
      console.log(index);
      this.tag.splice(index, 1);
    },
    confirmcustom() {
      function isEmpty(str) {
        return str.replace(/^\s+|\s+$/g, "").length == 0;
      }

      if (isEmpty(this.customtag)) {
        alert("input the customtag");
      } else if (this.customtag.length > 20) {
        alert("customtag must be 20 character");
      } else {
        this.tag.push(this.customtag);
      }
    },
  },
  mounted() {
    axios
      .get("https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/specificpost/" + this.$route.params.id,{withCredentials: false})
      .then((res) => {
        console.log(res.data.Item)
        this.oldpost = res.data.Item
        this.title = res.data.Item.title;
        this.description = res.data.Item.description;
        for (var i = 0; i < res.data.Item.tag.length; i++) {
          this.tag.push(res.data.Item.tag[i]);
        }
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style scoped></style>
