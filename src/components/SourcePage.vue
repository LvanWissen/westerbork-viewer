<template>
  <transition name="fade">
    <article v-show="isOpen">
      <CloseButton
        id="close-button"
        icon-color="#333333"
        @click.native="close"
      />
      <div class="content">
        <div
          class="media-container"
          v-html="media"
        />
        <section class="text-container">
          <h2>
            {{
              `${narrative.title.toUpperCase()} > ${$t(room).toUpperCase()} > ${$t(
                layer
              ).toUpperCase()}`
            }}
          </h2>
          <div
            class="text"
            v-html="content"
          />
        </section>
      </div>
    </article>
  </transition>
</template>

<script>
import CloseButton from './CloseButton.vue';

export default {
  name: 'SourcePage',
  components: {
    CloseButton,
  },
  props: {
    narrative: {
      type: Object,
      required: true,
    },
    room: {
      type: String,
      required: true,
    },
    sources: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isOpen: false,
      layer: 'house',
    };
  },
  computed: {
    media() {
      return this.sources[this.layer].media;
    },
    content() {
      return this.sources[this.layer].content;
    },
  },
  methods: {
    close() {
      this.isOpen = false;
    },
    open(layer) {
      this.layer = layer;
      this.isOpen = true;
    },
  },
};
</script>

<style scoped>
article {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(69, 69, 69, 0.95);
  color: #ffffff;
  overflow: auto;
  z-index: 9;
  display: flex;
  justify-content: center;
}

h2 {
  font-size: 1.125rem;
}

#close-button {
  position: absolute;
  top: 2rem;
  right: 3rem;
}

.content {
  max-width: 97.625rem;
  width: 90%;
  padding: 1rem;
  display: flex;
}

.media-container {
  padding: 3rem;
  width: 70%;
}

article >>> .media-container img {
  width: 100%;
}

article >>> .media-container video {
  width: 100%;
}

.text-container {
  padding: 3rem 0;
  width: 30%;
  line-height: 1.6rem;
}

@media only screen and (max-width: 1400px) {
  .content {
    flex-wrap: wrap;
    justify-content: center;
  }

  .text-container {
    width: 70%;
  }

  .media-container {
    width: 90%;
  }
}
</style>
