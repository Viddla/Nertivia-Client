<template>
  <div :key="avatar" class="outer-profile-picture">
    <div
      :class="`profile-picture ${admin && adminType ? adminType.name : ''}`"
      :style="{padding: animationPadding || '3px'}"
    >
      <!-- :style="`padding: ${$props.animationPadding || '3px'}`" -->
      <img
        v-if="adminType"
        class="emote"
        :src="adminType.emotePath"
        :style="
          `width: ${$props.emoteSize || '20px'}; height: ${$props.emoteSize ||
            '20px'}`
        "
      />

      <img
        v-if="src"
        class="inner-profile-picture"
        :class="{ hoverable: $props.hover }"
        :style="{
          height: $props.size,
          background: color,
          width: $props.size,
          border: `${
            statusColor === undefined
              ? undefined
              : 'solid 3px' + statusColor.statusColor
          }`
        }"
        :src="src + (hover || !isGif ? '' : '?type=webp')"
      />
      <div
        v-else
        class="inner-profile-picture default"
        :class="{ hoverable: $props.hover }"
        :style="{
          height: $props.size,
          backgroundColor: color,
          width: $props.size,
          border: `${
            statusColor === undefined
              ? undefined
              : 'solid 3px' + statusColor.statusColor
          }`
        }"
        :src="src + (hover || !isGif ? '' : '?type=webp')"
      />
    </div>
  </div>
</template>

<script>
import statuses from "@/utils/statuses";
import config from "@/config";
import seedColor from "seed-color";
export default {
  props: [
    "avatar",
    "size",
    "emoteSize",
    "admin",
    "hover",
    "animationPadding",
    "status",
    "uniqueID",
    "url"
  ],
  data() {
    return {
      nertiviaCDN: config.nertiviaCDN,
      color: !this.avatar ? seedColor(this.uniqueID).toHex() : undefined
    };
  },
  computed: {
    src() {
      if (!this.avatar && !this.url) {
        return undefined;
      }
      if (this.url) {
        return this.url;
      }
      return config.nertiviaCDN + this.avatar;
    },
    isGif() {
      if (!this.avatar) return undefined;
      return this.avatar.endsWith(".gif");
    },
    defaultImage() {
      // console.log(require("../../assets/logo.png"))
      return require("../../assets/transparentLogo.svg");
    },
    adminType() {
      if (this.$props.admin == 3)
        return {
          name: "creator",
          emotePath: require("../../assets/twemoji/1f451.svg")
        };
      if (this.$props.admin == 4)
        return {
          name: "cute",
          emotePath: require("../../assets/twemoji/1f33a.svg")
        };
      if (this.$props.admin == 5)
        return {
          name: "supporter",
          emotePath: require("../../assets/twemoji/2764.svg")
        };
      if (this.$props.admin == 6)
        return {
          name: "developer",
          emotePath: require("../../assets/twemoji/2728.svg")
        };
      return "";
    },
    statusColor() {
      let status = this.status;
      if (status === undefined) return undefined;
      return {
        statusName: statuses[parseInt(status)].name,
        statusURL: statuses[parseInt(status)].url,
        statusColor: statuses[parseInt(status)].color,
        bgColor: statuses[parseInt(status)].bgColor
      };
    }
  }
};
</script>

<style scoped>
.default {
  background-image: url("../../assets/transparentLogoProfile.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: 100%;
}
.outer-profile-picture {
  z-index: 999;
  user-select: none;
}
.profile-picture {
  position: relative;
  border-radius: 50%;
  flex-shrink: 0;
}
.inner-profile-picture {
  border-radius: 50%;
  object-fit: cover;
  display: flex;
  transition: 0.2s;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  -webkit-transform: translateZ(0) scale(1, 1);
  transform: translateZ(0);
}
.hoverable {
  cursor: pointer;
}
.hoverable:hover {
  filter: brightness(80%);
}
.creator {
  margin-right: 5px;
  margin-left: 0;
  flex-shrink: 0;
  margin: auto;
  background: #ffd828 100%;
}

.supporter {
  margin-right: 5px;
  margin-left: 0;
  flex-shrink: 0;
  margin: auto;
  background: #ff2828 100%;
}

.supporter .emote {
  z-index: 999;
  top: -3px;
  left: -3px;
}
.supporter .emote:after {
  z-index: 999;
}

.cute {
  margin-right: 5px;
  margin-left: 0;
  flex-shrink: 0;
  margin: auto;
  background: #ffb7ed;
}

.cute .emote {
  z-index: 999;
  top: -3px;
  left: -3px;
}

.developer {
  background: linear-gradient(30deg, #6853b9, rgba(254, 94, 189, 0.8));
}
.developer .emote {
  z-index: 999;
  top: -3px;
  left: -3px;
}

.emote {
  position: absolute;
  height: 20px;
  width: 20px;
  top: -6px;
  left: -6px;
  z-index: -1;
  opacity: 0;
}
.outer-profile-picture:hover .emote {
  animation-name: emote;
  animation-duration: 0.2s;
  animation-fill-mode: forwards;
}
@keyframes emote {
  0% {
    opacity: 0;
    transform: rotate(-40deg);
  }
  100% {
    opacity: 1;
    transform: rotate(-40deg);
  }
}
</style>
 