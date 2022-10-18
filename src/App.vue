<template>
  <div class="hero min-h-screen" style="background-image: url(../images/background.gif);">
    <div class="hero-overlay bg-opacity-20"></div>
    <div class="hero-content text-center text-neutral-content w-full">
      <div class="card w-full bg-base-100 text-primary-content border-primary">
        <div class="card-body">
          <h1 class="mb-6 text-4xl text-white font-bold uppercase">{{ appName }}</h1>
          <div class="card-actions mb-3 justify-center">
           <input v-model="inputDiscordId" type="text" placeholder="Discord ID" class="input input-bordered input-primary w-full max-w-xs" />
          </div>
          <div class="card-actions justify-center">
            <button @click="fetchUserInfoFromApi(inputDiscordId)" class="btn btn-primary">Steal Avatar</button>
          </div>

          <div v-if="showUser" class="user-info mt-12 w-full">
            <div class="user-body w-full">
              <div class="avatar justify-center w-full">
                <div class="w-24 rounded-full ring ring-primary ring-offset-base-100 ring-offset-2">
                  <img :src="avatarLink" />
                </div>
              </div>
              <div class="w-full">
                <h2 class="h2 text-center underline underline-offset-4 text-white text-3xl text-left pt-3 pb-10">{{username}}{{"#" + userIdentNum}}</h2>
              </div>
              <div class="w-full flex">
                <div class="w-1/2 flex justify-center">
                  <img :src="avatarLink">
                </div>
                <div class="w-1/2 flex justify-center">
                   <img :src="bannerLink">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appName: "Discord Avatar Thief",
      avatarLink: "",
      bannerLink: "",
      username: "",
      userIdentNum: "",
      inputDiscordId: "",
      showUser: false,
    }
  },
  methods: {
    async fetchUserInfoFromApi(inputDiscordId) {
      const baseUrl = 'http://localhost:8080/api';
      fetch(`${baseUrl}/getUser/${inputDiscordId}`)
      .then(response => response.json())
      .then(data => {
        this.avatarLink = data.avatarUrl;
        this.bannerLink = data.bannerUrl;
        this.username = data.username;
        this.userIdentNum = data.userIdentNum;
        this.bannerColor = data.bannerColor;
        this.showUser = true;
      })
    }
  },
}
</script>


<style>
  .border-primary {
    border: 1px hsl(var(--p)) solid;
  }

  .input {
    color: white;
  }
  
  .h2 {
    word-break: break-all;
  }

  .h2.underline {
    text-decoration-color: hsl(var(--p));
  }


</style>