<template>
  <div class="drop-down-menu" v-click-outside="outsideClick">
    <div class="item" @click="copyMessage" >Copy</div>
    <div class="item" @click="editMessage">Edit</div>
    <div class="item warn" @click="deleteMessage">Delete</div>
  </div>
</template>


<script>
import messagesService from '@/services/messagesService';
export default {
  data() {
    return {

    };
  },
  methods: {
    closeMenu() {
      this.$store.dispatch('setMessageContext', {messageID: null, x: null, y: null});
    },
    outsideClick(event) {
      const target = event.target;
      if (target.closest('.drop-down-button')) return;
      this.closeMenu();
    },
    editMessage() {
      this.$store.dispatch("setEditMessage", {
        channelID: this.contextDetails.channelID,
        messageID: this.contextDetails.messageID,
        message: this.contextDetails.message
      });
      this.closeMenu();
    },
    copyMessage() {
      this.$clipboard(this.contextDetails.message);
      this.closeMenu();
    },
    async deleteMessage() {
      messagesService.delete(this.contextDetails.messageID, this.contextDetails.channelID);
      this.closeMenu();
    },
    setPosition() {
      const y = this.contextDetails.y + 20;
      const x = this.contextDetails.x - 20;

      this.$el.style.top = y  + "px";
      this.$el.style.left = x + "px";
    }
  },
  mounted() {
    this.setPosition();
  },
  watch: {
    contextDetails() {
      this.setPosition();
    }
  },
  computed: {
    contextDetails() {
      const { x, y, messageID, message, channelID } = this.$store.getters.popouts.messageContextMenu;
      return { 
        x,
        y,
        messageID,
        message,
        channelID
      }
    }
  },

};
</script>


<style lang="scss" scoped>
.drop-down-menu {
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(31, 31, 31, 0.895);
  z-index: 99999;
  padding: 5px;
  border-radius: 5px;
  user-select: none;
  color: white;
}

.item {
  padding: 3px;
  margin: 2px;
  border-radius: 5px;
  transition: 0.3s;
  font-size: 13px;
  cursor: pointer;
  &:hover {
    background: rgb(56, 56, 56);
  }
  &.warn {
    &:hover {
      background: rgba(255, 90, 90, 0.338);
    }
    background: rgba(255, 90, 90, 0.1);
    color: rgb(255, 59, 59);
  }
}

</style>