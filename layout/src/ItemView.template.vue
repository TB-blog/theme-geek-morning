<style lang="stylus">
@import '../../source/style/ItemView.styl';
</style>

<template>
  <div class="item-view" v-if="item">
    <template v-if="item">
      <div class="item-view-header">
        <h1>{{ item.title }}</h1>
        <span v-if="item.labels" class="labels" v-for="label in item.labels" :key="label.id">
          <a :href="labelUrl + label.name"
            target="_blank"
            rel="noopener">#{{ label.name }}</a>
        </span>
        <p class="meta">
          Post By
          <a :href="`https://github.com/${this.$_config.user}`" target="_blank" rel="noopener">
            {{this.$_config.nickname}}
          </a>
          {{ item.created_at | timeAgo }}
        </p>
      </div>
      <div v-if="item.body" class="content" v-html="articleResource" v-highlight></div>
      <div class="item-view-footer">
        <p class="meta">
          Updated {{ item.updated_at | timeAgo }}
        </p>
      </div>
      <div class="item-view-comments">
        <div class="comments-content content" v-show="open && item.comments">
          <li v-for="comment in comments" :key="comment.id">
            <a class="avatar" :href="comment.user.html_url"
              target="_blank"
              rel="noopener">
              <img :src="comment.user.avatar_url" alt="comments-avatar">
            </a>
            <div class="body">
              <a :href="comment.user.html_url" target="_blank" rel="noopener">
                {{ comment.user.login }}
              </a>
              <span>Created {{ comment.created_at | timeAgo }}</span>
              <div class="markdown-body" v-html="comment.body" v-highlight>
              </div>
            </div>
          </li>
        </div>
        <a v-show="!open && item.comments" @click="open = !open" class="button">View comments({{ item.comments }})</a>
        <a v-show="!item.comments"
          class="button"
          :href="`https://github.com/${this.$_config.user}/${this.$_config.repo}/issues/${item.number}#new_comment_field`"
          target="_blank"
          rel="noopener">Add new comment</a>
        <a v-show="open && item.comments"
          class="button"
          :href="`https://github.com/${this.$_config.user}/${this.$_config.repo}/issues/${item.number}#new_comment_field`"
          >Add comments</a>
      </div>
    </template>
  </div>
</template>

<script>
import 'highlight.js/styles/agate.css';
export default {
  data () {
    return {
      open: false
    }
  },
}
</script>
