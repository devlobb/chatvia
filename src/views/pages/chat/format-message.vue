<script>
import formatString from "../../utils/formatString";

export default {
  props: {
    content: { type: [String, Number], required: true },
    deleted: { type: Boolean, default: false },
    formatLinks: { type: Boolean, default: true },
    singleLine: { type: Boolean, default: false },
    textFormatting: { type: Boolean, required: true },
  },
  computed: {
    linkifiedMessage() {
      return formatString(this.content, this.formatLinks);
    },
  },
  methods: {
    checkType(message, type) {
      return message.types.indexOf(type) !== -1;
    },
  },
};
</script>

  
<template>
  <div :class="{
			'text-ellipsis': singleLine
		}">
    <div v-if="textFormatting">
      <template v-for="(message, i) in linkifiedMessage">
        <component
          :is="checkType(message, 'url') ? 'a' : 'span'"
          :key="i"
          :class="{
						'text-ellipsis': singleLine,
						'text-deleted': deleted,
						'text-bold': checkType(message, 'bold'),
						'text-italic': checkType(message, 'italic'),
						'text-strike': checkType(message, 'strike'),
						'text-underline': checkType(message, 'underline'),
						'text-inline-code':
							!singleLine && checkType(message, 'inline-code'),
						'text-multiline-code':
							!singleLine && checkType(message, 'multiline-code')
					}"
          :href="message.href"
          target="_blank"
        >
          <i class="ri-delete-back-2-fill icon-deleted" v-if="deleted" name="deleted"></i>
          {{
          message.value
          }}
        </component>
      </template>
    </div>
    <div v-else>{{ content }}</div>
  </div>
</template>


<style>
.text-deleted {
  font-style: italic;
}
.icon-deleted {
  height: 14px;
  width: 14px;
  vertical-align: middle;
  margin: -3px 1px 0 0;
  fill: var(--chat-room-color-message);
}
.text-ellipsis {
  width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>