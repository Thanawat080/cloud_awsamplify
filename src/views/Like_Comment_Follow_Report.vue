<template>
  <div class="container">
    <div class="col-12">
      <h2>{{ post.title }}</h2>
      <p
        style="
          padding: 12px;
          overflow-wrap: break-word;
          word-wrap: break-word;
          hyphens: auto;
        "
      >
        {{ post.description }}
      </p>
      <div class="row">
        <div class="col-3">
          <img
            :src=img
            class="rounded float-start"
            width="200px"
            height="200px"
          />
        </div>
        
      </div>
      <p>Comment :</p>
      <div class="row" v-for="(value, index) in allcomment" :key="index">
        <div class="col-1">
          <img
            src="https://bahmansport.com/media/com_store/images/empty.png"
            style="width: 70px; height: 70px; border-radius: 360px;"
          />
        </div>
        <div class="col-10">
          <div class="row">
            <b>{{ value.first_name }} {{ value.last_name }}</b>
            <label for="">@ {{ value.email }}</label>
          </div>
          <div
            class="row"
            style="
              overflow-wrap: break-word;
              word-wrap: break-word;
              hyphens: auto;
            "
          >
            <div class="col-12">
              &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;{{ value.comment }}
            </div>
          </div>
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col-11" id="love" @click="like()">
          <svg
            width="22"
            height="22"
            fill="currentColor"
            class="bi bi-heart"
            viewBox="0 0 16 16"
            :style="{ color: color }"
          >
            <path
              d="m8 2.748-.717-.737C5.6.281 2.514.878 1.4 3.053c-.523 1.023-.641 2.5.314 4.385.92 1.815 2.834 3.989 6.286 6.357 3.452-2.368 5.365-4.542 6.286-6.357.955-1.886.838-3.362.314-4.385C13.486.878 10.4.28 8.717 2.01L8 2.748zM8 15C-7.333 4.868 3.279-3.04 7.824 1.143c.06.055.119.112.176.171a3.12 3.12 0 0 1 .176-.17C12.72-3.042 23.333 4.867 8 15z"
            />
          </svg>
          <span>&nbsp;&nbsp;&nbsp;{{ countlike }}</span>
        </div>
        <div class="col-1" id="report" data-toggle="modal" data-target="#exampleModalCenter">
          <svg
            width="22"
            height="22"
            fill="currentColor"
            class="bi bi-exclamation-triangle"
            viewBox="0 0 16 16"
          >
            <path
              d="M7.938 2.016A.13.13 0 0 1 8.002 2a.13.13 0 0 1 .063.016.146.146 0 0 1 .054.057l6.857 11.667c.036.06.035.124.002.183a.163.163 0 0 1-.054.06.116.116 0 0 1-.066.017H1.146a.115.115 0 0 1-.066-.017.163.163 0 0 1-.054-.06.176.176 0 0 1 .002-.183L7.884 2.073a.147.147 0 0 1 .054-.057zm1.044-.45a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566z"
            />
            <path
              d="M7.002 12a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 5.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995z"
            />
          </svg>
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col-11">
          <div class="form-floating">
            <textarea
              class="form-control"
              placeholder="Leave a comment here"
              id="floatingTextarea"
              v-model="comment"
            ></textarea>
            <label for="floatingTextarea">Comments</label>
          </div>
        </div>
        <div class="col-1" id="ment" @click="confirmcomment()">
          <svg
            width="26"
            height="26"
            fill="currentColor"
            class="bi bi-send"
            viewBox="0 0 16 16"
          >
            <path
              d="M15.854.146a.5.5 0 0 1 .11.54l-5.819 14.547a.75.75 0 0 1-1.329.124l-3.178-4.995L.643 7.184a.75.75 0 0 1 .124-1.33L15.314.037a.5.5 0 0 1 .54.11ZM6.636 10.07l2.761 4.338L14.13 2.576 6.636 10.07Zm6.787-8.201L1.591 6.602l4.339 2.76 7.494-7.493Z"
            />
          </svg>
        </div>
      </div>
      <br />
      <br />
      <br />

</div>
<!-- modal -->
<div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLongTitle">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <input type="text" class="form-control" name="" id="" v-model="reportmodel" >
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-warning" @click="confrimreport()" data-dismiss="modal">Confirm report</button>
      </div>
    </div>
  </div>
</div>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      post: [],
      comment: '',
      first_name: '',
      last_name: '',
      phone_number: '',
      userid: '',
      email: '',
      allcomment: [],
      color: '',
      alllike: [],
      find: 0,
      countlike: 0,
      allreport: [],
      reportmodel: '',
      img: ''
    }
  },
  methods: {
    postoruserinfo(id) {
      axios
        .get("https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/specificpost/" + id,{withCredentials: false})
        .then((res) => {
          console.log(res.data)
          this.post = res.data.Item
        })
        .catch((err) => {
          console.log(err)
        })
    },
    confirmcomment() {
      this.allcomment.push({
        first_name: this.first_name,
        last_name: this.last_name,
        phone_number: this.phone_number,
        userid: localStorage.getItem('id'),
        comment: this.comment,
        email: this.email,
      })
      axios
        .put('https://ou03vrids2.execute-api.us-east-1.amazonaws.com/comment/' + this.$route.params.id, {
          comment: this.allcomment,
        },{withCredentials: false})
        .then(() => {
          axios
            .get('https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/specificpost/' + this.$route.params.id,{withCredentials: false})
            .then((res) => {
              this.allcomment = res.data.Item.comment
            })
            .catch((err) => {
              console.log(err)
            })
        })
        .catch((err) => {
          console.log(err)
        })
    },
    like() {
      if (this.alllike.length > 0) {
        for (let i = 0; i < this.alllike.length; i++) {
          if (this.alllike[i].userid == localStorage.getItem('id')) {
            this.find = 1
            if (this.alllike[i].like) {
              this.alllike[i].like = false
              this.color = 'black'
              this.countlike -= 1
            } else {
              this.alllike[i].like = true
              this.color = 'pink'
              this.countlike += 1
            }
          }
        }
      }
      if (this.find == 0) {
        this.countlike += 1
        this.color = 'pink'
        this.alllike.push({ like: true, userid: localStorage.getItem('id') })
        this.find = 1
      }
      axios
        .put("https://ou03vrids2.execute-api.us-east-1.amazonaws.com/like/" + this.$route.params.id, {
          like: this.alllike,
        },{withCredentials: false})
        .then(() => {})
        .catch((err) => {
          console.log(err)
        })
    },
    confrimreport(){
      this.allreport.push({report: this.reportmodel,userid: localStorage.getItem('id') })
      axios
        .put('https://ou03vrids2.execute-api.us-east-1.amazonaws.com/report/' + this.$route.params.id, {
          report: this.allreport,
        },{withCredentials: false})
        .then(() => {
          axios
            .get('https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/specificpost/' + this.$route.params.id,{withCredentials: false})
            .then((res) => {
              this.allreport = res.data.Item.report
            })
            .catch((err) => {
              console.log(err)
            })
            this.reportmodel = ''
        })
        .catch((err) => {
          console.log(err)
        })
        this.isactive = false
    }
  },
  created() {
    axios
      .get("https://6mus9gbr73.execute-api.us-east-1.amazonaws.com/specificpost/" + this.$route.params.id ,{withCredentials: false})
      .then((res) => {
        this.allcomment = res.data.Item.comment
        this.alllike = res.data.Item.like
        this.allreport = res.data.Item.report
        console.log(res.data.Item.imageUrl)
        this.img = res.data.Item.imageUrl

        for (let i = 0; i < this.alllike.length; i++) {
          if (this.alllike[i].like) {
            this.countlike += 1
          }
          if (this.alllike[i].userid == localStorage.getItem('id')) {
            if (this.alllike[i].like) {
              this.color = 'pink'
            }
          }
        }
      })
      .catch((err) => {
        console.log(err)
      })
    axios
      .get('https://l5r8hpbor7.execute-api.us-east-1.amazonaws.com/specificuser/' + localStorage.getItem('id'),{withCredentials: false})
      .then((res) => {
        this.first_name = res.data.Item.first_name
        this.last_name = res.data.Item.last_name
        this.phone_number = res.data.Item.phone_number
        this.userid = res.data.Item._id
        this.email = res.data.Item.email
      })
      .catch((err) => {
        console.log(err)
      })
    this.postoruserinfo(this.$route.params.id)
  },
}

</script>
<style>
#ment:hover {
  cursor: pointer;
}
#love:hover {
  cursor: pointer;
}
#report:hover {
  cursor: pointer;
}
.modal-backdrop {
    z-index: 1040 !important;
    display: none;  
}
.modal-content {
    margin: 2px auto;
    z-index: 1100 !important;
}

</style>

