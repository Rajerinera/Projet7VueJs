<template>
  <div class="media">
    <h1>L'actualité du reséau!</h1>
    <div
      class="media-body"
      v-for="comment in comments.slice().reverse()"
      :key="comment.idcom"
    >
      <div class="mask">
        <h2 class="media-heading">{{ comment.title }}</h2>
        <p>{{ comment.content }}</p>
        <img v-if="comment.image !== null" :src="comment.image" />
        <img v-else src="../assets/image/icon.png" />

        <div>
          Publiée par <b>{{ comment.author }}</b>
        </div>
        <ul class="list-unstyled list-inline media-detail pull-right">
          <li>
            <button
              class="btn btn-danger"
              v-if="idcom == comment.idcom || user.admin == 1"
              @click="deleteCom(comment)"
            >
              Supprimer
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
let iduser1 = localStorage.getItem("userChoice");
import { mapState } from "vuex";
export default {
  name: "comment",
  data() {
    return {
      title: "",
      name: "",
      image: "",
      idcom: iduser1,
      comments: [],
      mask: true,
    };
  },
  computed: {
    ...mapState(["user"]),
  },
  mounted() {
    this.comments = [];
    axios
      .get("http://localhost:3000/api/comments", {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("userToken"),
        },
      })
      .then((response) => {
        (this.comments = response.data), console.log(this.comments);
        console.log(response);
      })
      .catch((error) => {
        console.log(error);
      }),
      this.$store.dispatch("getInfo");
  },
  methods: {
    async deleteCom(comment) {
      const deleteComment = window.confirm("Etes vous sûr?");
      if (deleteComment == true) {
        console.log(comment.idcomment + "ceci");
        await axios
          .delete("http://localhost:3000/api/comment/" + comment.idcomment, {
            headers: {
              Authorization: "Bearer " + localStorage.getItem("userToken"),
            },
          })
          .then((response) => {
            console.log(response);
            console.log("supprimer fait");
            location.reload();
          })
          .catch((error) => console.log(error));
      }
    },
  },
};
</script>
<style scoped>
.mask {
  border: 1px solid black;
}
img {
  width: 70%;
  height: 50%;
  margin-left: auto;
  margin-right: auto;
  border: 2px solid black;
  object-fit: cover;
  background-color: blanchedalmond;
}
h1 {
  margin-left: auto;
  margin-right: auto;
  padding-top: 20px;
  padding-bottom: 50px;
  width: 100%;
  border-top: 1px solid black;
}
.media {
  width: 100%;
  margin-right: auto;
  margin-left: auto;
  padding: 25px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-bottom: 20px;
  background-image: url("../assets/image/wallpaper.webp");
  background-repeat: round;
}
.media-body {
  display: flex;
  flex-direction: column;
  text-align: center;
  margin-right: auto;
  margin-left: auto;
  width: 90%;
  padding: 15px;
}
.content-item {
  padding: 30px 0;
  background-color: #ffffff;
}

.content-item.grey {
  background-color: #f0f0f0;
  padding: 50px 0;
  height: 100%;
}

.content-item h2 {
  font-weight: 700;
  font-size: 35px;
  line-height: 45px;
  text-transform: uppercase;
  margin: 20px 0;
}

.content-item h3 {
  font-weight: 400;
  font-size: 20px;
  color: #555555;
  margin: 10px 0 15px;
  padding: 0;
}

.content-headline {
  height: 1px;
  text-align: center;
  margin: 20px 0 70px;
}

.content-headline h2 {
  background-color: #ffffff;
  display: inline-block;
  margin: -20px auto 0;
  padding: 0 20px;
}

.grey .content-headline h2 {
  background-color: #f0f0f0;
}

.content-headline h3 {
  font-size: 14px;
  color: #aaaaaa;
  display: block;
}

#comments {
  box-shadow: 0 -1px 6px 1px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
}

#comments form {
  margin-bottom: 30px;
}

#comments .btn {
  margin-top: 7px;
}

#comments form fieldset {
  clear: both;
}

#comments form textarea {
  height: 100px;
}

#comments .media {
  border-top: 1px dashed #dddddd;
  padding: 20px 0;
  margin: 0;
}

#comments .media > .pull-left {
  margin-right: 20px;
}

#comments .media img {
  max-width: 20px;
}

#comments .media h4 {
  margin: 0 0 10px;
}

#comments .media h4 span {
  font-size: 14px;
  float: right;
  color: #999999;
}

#comments .media p {
  margin-bottom: 15px;
  text-align: justify;
}

#comments .media-detail {
  margin: 0px;
}
.media-detail {
  margin: 12px;
}
#comments .media-detail li {
  color: #aaaaaa;
  font-size: 12px;
  padding-right: 10px;
  font-weight: 600;
}

#comments .media-detail a:hover {
  text-decoration: underline;
}

#comments .media-detail li:last-child {
  padding-right: 0;
}

#comments .media-detail li i {
  color: #666666;
  font-size: 15px;
  margin-right: 10px;
}
img.media-object {
  border: 1px solid black;
  width: 50px;
  height: 50px;
}
</style>
