<template lang="pug">
router-link.text-decoration-none.text-black(:to="`/chat/${chatRoomInfo.key}`")
    div(
        :class="['chat-room', 'd-flex', 'flex-row', 'align-items-center', 'gap-3', 'p-2', 'rounded-pill', isThisChatRoomActive ? 'active' : '']",
        :id="`chat-room-${chatRoomInfo.key}`",
        @animationend="animationEnd"
    )
        ChatIcon(:chatRoomInfo="chatRoomInfo", :size="3")
        .d-flex.flex-column
            p.h5.mb-0 {{ chatRoomInfo.roomName }}
            span.text-muted.mb-0.last-message {{ chatRoomInfo.lastMessage }}
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ChatIcon from "@/components/ChatIcon.vue";
import { useChatStore } from "@/stores/chat";
import { useChatRoomStore } from "@/stores/chatRoom";
import type ChatRoomInfo from "@/interfaces/chatRoomInfo";

export default defineComponent({
    components: { ChatIcon },
    props: {
        chatRoomInfo: {
            type: Object as () => ChatRoomInfo,
            required: true,
        },
    },
    data(): {
        animated: boolean;
        chatRoomStore: ReturnType<typeof useChatRoomStore>;
        chatStore: ReturnType<typeof useChatStore>;
    } {
        return {
            animated: false,
            chatRoomStore: useChatRoomStore(),
            chatStore: useChatStore(),
        };
    },
    computed: {
        isThisChatRoomActive() {
            return this.chatStore.selectedRoom?.key === this.chatRoomInfo.key;
        },
    },
    methods: {
        animationEnd() {
            let chatElement = document.getElementById(
                `chat-room-${this.chatRoomInfo.key}`
            );
            chatElement?.classList.remove(
                "animate__animated",
                "animate__slideInLeft",
                "animate__faster"
            );
            this.chatRoomStore.animated = true;
        },
    },
    mounted() {
        if (!this.chatRoomStore.animated) {
            let chatElement = document.getElementById(
                `chat-room-${this.chatRoomInfo.key}`
            );
            chatElement?.classList.add(
                "animate__animated",
                "animate__slideInLeft",
                "animate__faster"
            );
        }
    },
});
</script>

<style lang="scss" scoped>
div.chat-room {
    transition: 0.25s background-color;
    &:hover {
        background-color: transparentize($color: gray, $amount: 0.9);
        cursor: pointer;
        user-select: none;
    }

    &.active {
        background-color: #0d6efd;
        color: white;
        span.last-message {
            color: transparentize($color: #fff, $amount: 0.4) !important;
        }
    }

    span.last-message {
        font-size: 0.8rem;
    }
}
</style>
