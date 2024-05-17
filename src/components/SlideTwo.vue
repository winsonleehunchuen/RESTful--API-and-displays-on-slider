<template>
  <div class="youtube-slider">
    <h2>Youtube Slider</h2>
    <div class="slider-container">
      <button @click="prevSlide" :disabled="currentIndex === 0" class="control-button prev-button">&laquo;</button>
      <div class="slider">
        <div class="slides-container">
          <div v-for="(post, index) in visiblePosts" :key="index" class="slide" :class="{ active: isActive(index) }">
            <button class="post-link" @click="navigateToLink(post.link)">
              <div class="image-container" v-for="(image, imageIndex) in post.image.slice(0, 3)" :key="imageIndex">
                <img :src="image" :alt="post.title" class="slide-image">
              </div>
              <div class="description">{{ post.title }}</div>
            </button>
          </div>
        </div>
      </div>
      <button @click="nextSlide" :disabled="currentIndex === numSlides - 1" class="control-button next-button">&raquo;</button>
    </div>
    <div v-if="error" class="error-message">{{ error }}</div>
  </div>
</template>
  
<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: [],
      currentIndex: 0,
      numSlides: 0,
      error: null
    };
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    fetchPosts() {
      axios.get('https://ahc-cdn.innity-asia.com/feed/b7fc3454-a5a0-4646-a583-86d6fe490a51.json')
        .then(response => {
          this.posts = response.data.item.slice(0, 5); // Limit to the first 5 items
          this.numSlides = Math.ceil(this.posts.length / 3);
          this.updateVisiblePosts();
        })
        .catch(() => {
          this.error = 'Error fetching not images found!' ;
        });
    },
    updateVisiblePosts() {
      const start = this.currentIndex * 3;
      this.visiblePosts = this.posts.slice(start, start + 3);
    },
    prevSlide() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.updateVisiblePosts();
      }
    },
    nextSlide() {
      if (this.currentIndex < this.numSlides - 1) {
        this.currentIndex++;
        this.updateVisiblePosts();
      }
    },
    navigateToLink(link) {
      window.location.href = link;
    },
    isActive(index) {
      return index >= this.currentIndex * 3 && index < (this.currentIndex + 1) * 3;
    }
  }
};
</script>
  
<style scoped>
.youtube-slider {
  width: 100%;
  max-width: 1200px;
  margin: 3% auto;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  font-size: 24px;
  color: #333;
}

.slider-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.slider {
  display: flex;
  overflow: hidden;
  width: 80%;
}

.slides-container {
  display: flex;
  transition: transform 0.5s ease;
}

.slide {
  border: 1px solid #D3D3D3;
  margin: 0 2%;
  flex: 0 0 calc(33.3333% - 4%);
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.slide.active {
  opacity: 1;
}

.image-container {
  overflow: hidden;
  border-radius: 8px 8px 0 0;
  height: 200px; /* Fixed height for the image container */
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.description {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  word-break: break-all;
  margin: 15px;
  text-align: center;
  font-size: 16px;
  color: #333;
}

.control-button {
  background-color: #333;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  height: 50px; /* Adjust the button height */
  display: flex;
  align-items: center;
  justify-content: center;
}

.control-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.prev-button {
  margin-right: 10px;
}

.next-button {
  margin-left: 10px;
}

button.post-link {
  cursor: pointer;
  background: #fff;
  border: none;
  width: 100%;
  /* height: 100%; */
  padding: 0;
}

@media (max-width: 768px) {
  .slides-container {
    flex-wrap: wrap;
  }
  .slide {
    flex: 0 0 100%;
  }
  .slider {
    width: 100%;
  }
}
</style>
