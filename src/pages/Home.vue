<script setup>
import { ref, onMounted } from "vue";
import Loading from "../components/Loading.vue";

const isLoading = ref(true);
const user = ref({});
const repos = ref([]);

const userUrl = "https://api.github.com/users/chinatul";
const repoUrl = "https://api.github.com/users/ChinatuL/repos?per_page=250";
const token = import.meta.env.VITE_GITHUB_TOKEN;

async function getUser() {
    const response = await fetch(userUrl, {
        headers: {
            Authorization: `token ${token}`,
        },
    });
    const data = await response.json();
    user.value = data;
    isLoading.value = false;
}

async function getRepos() {
    const response = await fetch(repoUrl, {
        headers: {
            Authorization: `token ${token}`,
        },
    });
    const data = await response.json();
    repos.value = data;
    isLoading.value = false;
}

onMounted(() => {
    getUser();
    getRepos();
});
</script>

<template>
    <Loading v-if="isLoading" />
    <main class="main flex-col">
        <section class="profile flex-row">
            <div class="flex-row intro">
                <div class="profile__img">
                    <img :src="user.avatar_url" alt="avatar" />
                    <div class="flex-col">
                        <h1 class="profile__fullname">
                            {{ user.name }}
                        </h1>
                        <p class="profile__username">
                            {{ user.login }}
                        </p>
                        <p class="profile__bio">{{ user.bio }}</p>
                    </div>
                </div>
            </div>
            <div class="flex-row intro2">
                <div class="profile__followers">
                    <img src="../assets/Icons/People.svg" alt="" />
                    <p>{{ user.followers }} followers</p>
                </div>
                <div class="profile__following">
                    <img src="../assets/Icons/People.svg" alt="" />
                    <p>{{ user.following }} following</p>
                </div>
                <div class="profile__location">
                    <img src="../assets/Icons/Location.svg" alt="" />
                    <p>{{ user.location }}</p>
                </div>
                <div class="profile__email">
                    <img src="../assets/Icons/Email.svg" alt="" />
                    <p>{{ user.email }}</p>
                </div>
                <div class="profile__twitter">
                    <img src="../assets/Icons/Twitter.svg" alt="" />
                    <p>@{{ user.twitter_username }}</p>
                </div>
                <div class="profile__repos">
                    <img src="../assets/Icons/Repo.svg" alt="" />
                    <p>{{ user.public_repos }} Repositories</p>
                </div>
            </div>
        </section>
        <section class="repos">
            <h2 class="repos__heading">Repositories</h2>
            <div class="repos__list">
                <a href="#" v-for="repo in repos" :key="repo.id" class="repo">
                    <article class="flex-col">
                        <div class="flex-row badges">
                            <span>{{ repo.visibility }}</span>
                            <img src="../assets/Icons/Link.svg" alt="" />
                        </div>
                        <div class="flex-col">
                            <h3 class="repo__title">{{ repo.name }}</h3>
                            <p class="repo__description">
                                {{ repo.description }}
                            </p>
                        </div>
                    </article>
                </a>
            </div>
        </section>
    </main>
</template>
