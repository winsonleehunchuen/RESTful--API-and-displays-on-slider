<template>
  <div class="car-bike-slider">
    <h2>Car & Bike Posts Slider</h2>
    <div class="slider-container">
      <button @click="prevSlide" :disabled="currentIndex === 0" class="prev-button">&laquo;</button>
      <div class="slider">
        <div class="slide" v-for="(post, index) in visiblePosts" :key="index" :class="{ active: index === currentIndex }">
          <div class="image-container" v-for="(image, index) in post.images" :key="index">
            <img :src="getImageUrl(image.ori)" :alt="post.title" class="slide-image">
          </div>
          <div class="description">{{ post.title }}</div>
        </div>
      </div>
      <button @click="nextSlide" :disabled="currentIndex === visiblePosts.length - 1" class="next-button">&raquo;</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: [],
      currentIndex: 0,
      visiblePosts: []
    };
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    fetchPosts() {
      axios.get('https://www.loopme.my/api/live-post-ajax?category=cars-bikes&sort=newest&page=1&paginate=20')
        .then(response => {
          this.posts = response.data.feed.data.posts;
          this.updateVisiblePosts();
        })
        .catch(error => {
          console.error('Error fetching posts:', error);
        });
    },
    updateVisiblePosts() {
      const startIndex = Math.max(0, this.currentIndex);
      this.visiblePosts = this.posts.slice(startIndex, startIndex + 3);
    },
    prevSlide() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.updateVisiblePosts();
      }
    },
    nextSlide() {
      if (this.currentIndex < this.posts.length - 1) {
        this.currentIndex++;
        this.updateVisiblePosts();
      }
    },
    getImageUrl(ori) {
      return `https://s3-ap-southeast-1.amazonaws.com/s3.loopme.my/img/newos/aiposts/${ori.path}`;
    }
  }
};
</script>

<style scoped>
.car-bike-slider {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.slider-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.slider {
  display: flex;
  overflow: hidden;
}

.slide {
  border: 1px solid #D3D3D3;
  flex: 0 0 auto;
  margin-right: 10px;
  width: 98%;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.image-container {
  overflow: hidden;
  border-radius: 8px 8px 0 0;
  width: 100%;
  height: 400px; /* Fixed height for the image container */
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure the image covers the container without distortion */
  display: block;
}

.description {
  overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
    /* word-break: break-all; */
    margin: 15px;
    text-align: center;
    font-size: 18px;
    color: #333;
}

button {
  background-color: #333;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.prev-button {
  margin-right: 1%;
}

@media (max-width: 768px) {
  .car-bike-slider {
    padding: 0 20px;
  }
}
</style>
